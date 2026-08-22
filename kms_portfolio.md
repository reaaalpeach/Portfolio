0# 📂 Project Portfolio | Client Develop
> **Unity 클라이언트 개발자 [김민선]입니다.**

## 📋 Table of Contents
1. [NCT ZONE - 콘텐츠 및 시스템 개발](#-project-1-nct-zone)
2. [Project V](#-project-2-project-v)

---

## 🎮 Project 1: NCT ZONE
> **NCT 멤버들과 함께하는 시네마틱 어드벤처 게임의 시스템 개발 및 라이브 서비스 개발/유지 보수**

<p align="left">
  <img src="https://github.com/user-attachments/assets/e0b2ed0a-a256-476b-a292-7db899c87c19" width="30%" alt="Icon">
  <img src="https://github.com/user-attachments/assets/acd39162-0200-4817-a3c2-37c869f588f6" width="30%" alt="NCT ZONE Main">
</p>

### 📌 Project Overview
- **개발 기간 :** 2022.11 ~ 2026.02
- **기술 스택 :** Unity 3D, C#, UGUI, UniRx
- **담당 역할 :** 클라이언트 컨텐츠 로직 구현 (스테이지 시스템, 미니게임, 덱, 빙고게임 등)
<br />

### ✨ Key Implementation

#### 1️⃣ **스테이지 & 챕터 시스템**
기존에 구축된 스테이지 시스템의 기본 구조를 바탕으로, 신규 게임 모드(하드, 이벤트) 확장 및 UI 연동/유지보수를 담당.
- **주요 구현 :** 
    - 챕터 간 이동 및 해금 조건 체크, 스테이지 진입 팝업 등의 UI 플로우 및 클라이언트 컨텐츠 로직 구현.
    - **모드별 확장 및 유지보수 :** 기존 Normal모드 중심의 구조에 하드 모드와 기간 한정 이벤트 스테이지를 확장하기 위한 로직 구현.
    - **이벤트 스테이지 제어 :** 기간 한정 컨텐츠를 위해 서버 시간과 연동된 활성화/비활성화 구현.
    - **모드별 독립성 확보 :** Enum 타입을 활용해 모드를 분류하고, 각 모드가 동일한 인터페이스를 공유하되 개별 기능은 각 모드에 맞게 동작하도록 설계.

<p align="left">
  <img src="https://github.com/user-attachments/assets/d4fa72dd-3297-4e29-a7a5-af74a20a3c82" width="30%" alt="Stage System">
  <img src="https://github.com/user-attachments/assets/1228ba7e-4f95-4cc0-88fc-b165f0c6278f" width="30%" alt="Worldmap">
  <img src="https://github.com/user-attachments/assets/54c252ec-b7c8-4ca5-9e3d-626c95a477d5" width="30%" alt="Stage Enter Popup">
</p>
 
- **개선/최적화:** 
    - 기존 시스템에서 하드 모드와 이벤트 스테이지가 추가됨에 따라 데이터 구조가 복잡해지는 문제가 발생.
    partial 키워드를 통해 class를 나누고 데이터 오염 방지와 유지보수 편의성을 동시에 확보. 

### 💻 Code Snippets
<details>
<summary><b> ▶ 모드별 데이터 구분을 위한 구조(클릭하여 펼치기) </b></summary>
  
    public enum CHAPTER_MODE
    {
        MODE_NORMAL,
        MODE_HARD,
        EVENT
    }
    
    public List<StageList> stage_list = new();              // 챕터의 모든 스테이지 리스트
    public List<ChapterList> chapterList = new();
    
    public partial class KwangyaManager : SingletonMono<KwangyaManager>
    {
        public void SetStageList(List<StageList> list) // 서버 스테이지 정보 셋팅
        {
            if (list != null)
            {
                stage_list?.Clear();

                list.ForEach(info =>
                {
                    if (GetMirrorStageData(info.gdid).mirrorstg_enable == (int)COMMON_AVAILABILLITY.BOOL_ENABLE)
                        stage_list.Add(info);
                    else
                        CDebug.Log($"Kwnagya SetStageList {info.gdid}", CDebugTag.KWANGYA);
                });
            }
        }
    
        public MirrorStageData GetMirrorStageData(int stage_id) // 스테이지에 따른 테이블 정보 가져오기
        {
            if (stage_id < 0)
                return null;

            return CMirrorDataManager.Instance.GetMirrorStageData(stage_id);
        }
    
        public StageList GetStage(int stage_id)
        {
            if (GetStageListByKwangyaType().Count <= 0)
                return null;

            StageList stage = new();
            GetStageListByKwangyaType().ForEach(info =>
            {
                if (info.gdid == stage_id)
                {
                    stage = info;
                }
            });

            return stage;
        }
    
        public List<StageList> GetStageListByKwangyaType() // 
        {
            if (kwangyaType == KWANGYA_TYPE.KWANGYA)
                return stage_list;
            else
                return evt_stage_list;
        }
    }
    
</details>    
<br />

#### 2️⃣ **덱 저장 및 편집 시스템**
유저가 스테이지 공략을 위해 설정한 최적의 카드 조합을 서버에 저장하고 불러오는 기능을 구현.
- **주요 구현 :**
    - 다중 덱 슬롯 관리 로직
    - 보스별 속성 상성과 카드의 상성에 따른 빠른 편성 기능 추가 

<p align="left">
  <img src="https://github.com/user-attachments/assets/a151e925-5279-43eb-b8ab-102365c4330d" width="30%" alt="Deck Editing1">
  <img src="https://github.com/user-attachments/assets/38589e76-9e1b-4eaf-ab39-4427816220d9" width="30%" alt="Deck Editing2">
</p>

- **개선/최적화:** 기존 시스템에서 타입별로 덱을 구성하게됨에 따라 데이터 구조가 복잡해지는 문제가 발생.
이를 해결하기 위해 상속을 통해 공통 로직은 재사용하되 타입별 특수 로직은 개별적으로 처리하도록 구현하여 데이터 오염 방지 및 유지 보수 편의성을 동시에 확보.

### 💻 Code Snippets
  
<details> <summary><b>▶ 1️⃣ 덱 데이터 구조 및 저장 로직 </b></summary>
   
        public class CardDeckEditing : CPopupUI<CardDeckEditing.Setting, CardDeckEditing.Result>
        {
            protected List<CardList> cardPacketList = new List<CardList>();
            protected List<int> deckPacketList = new List<int>();
            protected List<CardList> curDeckCardList = new List<CardList>(); // 현재 열려있는 덱에 있는 카드리스트
            protected Dictionary<int, List<CardList>> deckCardDic = new Dictionary<int, List<CardList>>(); // 덱이 여러개일 경우 덱마다 카드리스트 관리

            protected int curDeckIndex = 0; // 덱이 여러개일 경우 해당 덱의 인덱스

            protected virtual void SetDeckList(DeckList deckList, List<DeckList> decksList = null)
            {
                List<DeckList> decks = new List<DeckList>();
                if (deckList != null) // 덱 1개일 경우
                {
                    if(deckList.character0 > 0)
                        decks.Add(deckList);
                }
                else
                    decks = decksList != null ? decksList : null;

                isServerDeckEmpty = decks == null || decks != null && decks.Count <= 0;

                if (isServerDeckEmpty)
                    SetTempDeckInfo();
                else
                {
                    for (int i = 0; i < decks.Count; i++)
                    {
                        var deck = decks[i];
                        SetDeckData(0, deck.character0, i);
                        SetDeckData(1, deck.character1, i);
                        SetDeckData(2, deck.character2, i);
                        SetDeckData(3, deck.character3, i);
                        SetDeckData(4, deck.character4, i);
                        SetDeckData(5, deck.character5, i);
                        SetDeckData(6, deck.character6, i);
                        SetDeckData(7, deck.character7, i);
                    }
                }
            }

            protected void SetDeckData(int index, int cardId, int deckIndex = 0, bool isInit = true)
            {
                CardList cardData = null;
                bool isSameWeakPoint = false;
                if (cardId > 0)
                {
                    cardData = cardPacketList.Find(c => c.gdid == cardId);
                    isSameWeakPoint = CheckWeakPoint(cardData.GetDPZTYPE(), deckIndex);
                }

                deck.SetData(index, cardData, this, OnClickCardEvent, isSameWeakPoint, cardDeckUseType, deckIndex);
                SetDeckCardDic(cardData, deckIndex);

                if(isInit)
                    deckPacketList.Add(cardId);
            }

            public virtual void OnSave()
            {
                if (IsBtnDisable)
                    return;

                var popup = popupService.NoticeAlert("Mirror_deck_full_pop_desc", PopupAlert.BUTTON_TYPE.BTN_TWO);

                SingleAssignmentDisposable dispose = new SingleAssignmentDisposable();
                dispose.Disposable = popup.ShowAsObservable()
                .Do(result =>
                {
                    if (result.IsSucess)
                    {
                        if (IsModified())
                        {
                            var saveData = GetServerSaveData();
                            ServerDeckSave(saveData);
                        }
                        else
                            OnClickClose();
                    }
                    dispose.Dispose();
                }).Subscribe().AddTo(this);
            }

            protected Dictionary<int, List<int>> GetServerSaveData()
            {
                int idCnt = 8;
                Dictionary<int, List<int>> deckIdDic = new Dictionary<int, List<int>>();
                for (int i = 0; i < deckCnt; i++)
                {
                    List<int> deckIdList = new List<int>();
                    if (deckCardDic.ContainsKey(i))
                    {
                        for (int k = 0; k < idCnt; k++)
                        {
                            var tempDeck = deck.GetDeckByIndex(k, i);
                            deckIdList.Add(tempDeck.Id);
                        }
                    }
                    else
                    {
                        for (int k = 0; k < idCnt; ++i)
                        {
                            deckIdList.Add(0);
                        }
                    }

                    deckIdDic.Add(i, deckIdList);
                }

                // 덱 최대갯수보다 현재 덱갯수가 작을 때, 패킷에 보낼 빈 덱 추가
                if (deckIdDic.Count < deckCntMax)
                {
                    int emptyDeck = deckIdDic.Count;
                    for (int i = emptyDeck; i < deckCntMax; i++)
                    {
                        deckIdDic.Add(i, null);
                    }
                }

                return deckIdDic;   
            }

            protected virtual void ServerDeckSave(Dictionary<int, List<int>> deckIdList) // 타입별로 덱 
            {

            }
        }

</details>

<details> <summary><b>▶ 2️⃣ 세부 덱 정보 </b></summary>

            public void SetData(CardList cardInfo, CardDeckEditing deckEditing, CardDeckEditing.EditingType editingType,
                    int index, bool isSameWeakPoint, CardDeckUseType useType, int deckIndex)
            {
                this.index = index;
                this.deckEditing = deckEditing;
                this.cardInfo = cardInfo;
                this.editingType = editingType;
                this.isSameWeakPoint = isSameWeakPoint;
                this.useType = useType;
                this.deckIndex = deckIndex;   

                inRectCard = null;

                if(infoRootGo != null)
                {
                    if (editingType == CardDeckEditing.EditingType.Deck)
                    {
                        IsInfoGo = Id > 0;
                        IsCardInfoOn = Id > 0;  
                        SetCoverBlack(false);
                    }

                    if (!IsActive)
                        IsActive = true;

                    UpdateUI();
                }
            }

</details>
<br />

#### 3️⃣ **빙고 이벤트 시스템**
미션 수행과 재화 소모를 결합하여 유저의 리텐션을 유도하는 이벤트 시스템을 개발.
- **주요 구현 :**
    - **이원화 도장 시스템 :** 미션 보상(일반 도장)과 유료 재화(스페셜 도장) 사용 로직 구현
    - **빙고 판정 알고리즘 :** 5x5 그리드의 가로, 세로, 대각선 완성 여부를 실시간 검출
    - **최종 보상 연동 :** 올빙고 달성 시 '미공개 사진' 뷰어 및 해금 연출 구현

<p align="left">
  <img src="https://github.com/user-attachments/assets/9f0f7031-4812-4b48-871d-17a51cb1a366" width="30%" alt="Bingo">
  <img src="https://github.com/user-attachments/assets/27f02cf7-d9c3-4ffe-96b9-24a15d355764" width="30%" alt="Bingo Line Clear">
  <img src="https://github.com/user-attachments/assets/775286aa-0a05-4f0b-939a-6ffbee3d3de0" width="30%" alt="Bingo Mission">
</p>
  
### 💻 Code Snippets

<details>
<summary><b>▶ 1️⃣ 슬롯 데이터 </b></summary>  
    
    public class ObjBingoSlot : MonoBehaviour
    {
        private BingoSlotData slotData = null;
        private PageBingoEvent PageBingoEvent;
    
        public void Init(BingoSlotData data, BINGO_SLOT_STATE _state, int _slotIdx, PageBingoEvent _pageBingoEvent) // 개별 빙고데이터 셋팅
        {
            PageBingoEvent = _pageBingoEvent;   
            slotData = data;
            state = _state;
            index = _slotIdx;

            rootObj.SetActive(state == BINGO_SLOT_STATE.CLOSED);
            lockObj.SetActive(data.slotType == BING0_SLOT_TYPE.BINGO_SLOT_LOCK);
            slotNumText.text = index.ToString();  
        }
    
        public void OpenSlot()
        {
            state = BINGO_SLOT_STATE.OPEN;

            var popupBingoCommonStamp = CCoreServices.GetInstance().GetService<CPopupService>().GetPopup<PopupBingoCommonStamp>();
            if (popupBingoCommonStamp != null)
                popupBingoCommonStamp.Close();

            PlayEffSound();
            openAnim.Play();

            StartCoroutine(PageBingoEvent.CompleteLine(index, openAnim));
        }
    }
</details>
    
<details> <summary><b>▶ 2️⃣ 라인 완성 체크 및 연출 </b></summary>
    
    public class PageBingoEvent : PagePopup
    {
        public IEnumerator CompleteLine(int slotIdx, Animation anim)
        {
            yield return new WaitUntil(() => !anim.isPlaying);

            var clearDic = GetClearLineBySlotIdx(slotIdx);
            if (clearDic.Count < 1)
            {
                Utility.SetFxLayerDimmed(false);
                yield break;
            }

            foreach (var dic in clearDic)
            {
                var rwdIndex = dic.Key - 1;
                var slotList = dic.Value;
                for (int i = 0; i < slotList.Count; i++)
                {
                    var slot = slotList[i];

                    slot.PlayOpenAnim();

                    if (i < slotList.Count -1)
                        yield return new WaitForSeconds(delay.slot);
                    else
                        yield return new WaitForSeconds(delay.lineRwd);
                }

                CSoundControl.Instance?.EffectPlay(SOUND_ID.SFX_BINGO_LINE);
                objLineRwdList[rwdIndex].SetState(BINGO_RWD_STATE.RWD_OPEN);

                yield return new WaitForSeconds(delay.line);
            }

            if (CheckBingoAllClear())
            {
                imgBlur.enabled = false;
                bingoLineOBj.SetActive(false);
                SetDesc();

                yield return new WaitForSeconds(delay.openPopup);

                OpenPopupBingolear();
            }
            else
                Utility.SetFxLayerDimmed(false);
        }
                                          
        private bool CheckBingoAllClear() // 빙고 전체 클리어 
        {
            bool isClear = true;
            for(int i = 0; i < curBingoData.slots.Count; i++)
            {
                var slot = curBingoData.slots[i];
                if (slot != (int)BINGO_SLOT_STATE.OPEN)
                {
                    isClear = false;
                    break;
                }
            }

            return isClear;
        }
    }
    
</details>
<br />

#### 4️⃣ **미니게임 2종**
게임 내 체류 시간을 높이기 위한 캐주얼 미니 게임 개발에 참여.

##### 🏎️ 레이싱 미니게임
- 게임 시작/오버 적용
- 인게임 내 각종 이펙트/애니메이션 적용
- 플레이 중 발생하는 전반적인 버그 수정

<p align="left">
  <img src="https://github.com/user-attachments/assets/a1831208-c4a1-4f19-9287-03ee7b57416b" width="30%" alt="Racing Main">
  <img src="https://github.com/user-attachments/assets/9fb61c6b-73c3-4291-a6fc-a3262edc72f6" width="30%" alt="Racing Playing">
</p>

##### 🍉 수박게임
- 오브젝트 풀링을 이용한 게임 최적화
- 원형 물리 충돌 판정을 이용한 오브젝트 합성 로직
- 상단 투하 위치 가이드 및 스코어 시스템
- 상단 종료 판정을 위한 데드라인 적용

<p align="left">
  <img src="https://github.com/user-attachments/assets/1cb8eb30-610c-4cba-9c07-3508d44c1f87" width="30%" alt="Watermelon Main">
  <img src="https://github.com/user-attachments/assets/6fb698bb-0fe1-43c5-91d7-ac95dbbad17e" width="30%" alt="Watermelon Playing">
  <img src="https://github.com/user-attachments/assets/61db136a-fc3c-42b5-a997-8c242cee2d04" width="30%" alt="Watermelon Select Item">
</p>

##### 미니게임 공통 적용 사항
- **아이템 시스템 :** 각기 다른 효과를 가진 아이템을 고유 트리거 시스템을 통해 사용 즉시 연동되는 특수 효과 및 로직을 구현하고, 게임의 템포를 조절하는 다양한 변수 창출.
- **랭킹 시스템 :** 전체 유저 대상의 '글로벌 랭킹'과 소셜 연동 정보를 활용한 '친구 랭킹' 데이터 분리 및 UI 대응.

<p align="left">
  <img src="https://github.com/user-attachments/assets/7fe135d3-d71b-49db-85bf-59ef9a5544df" width="30%" alt="MiniGame Ranking System">
</p>

- **개선/최적화:** 다양한 아이템 연출이 겹칠 때 프레임 드랍이 발생하는 문제를 방지하기 위해, 이펙트 오브젝트에도 오브젝트 풀링을 적용하여 안정적인 성능을 유지.

### 💻 Code Snippets  
#### 🏎️ 레이싱 미니게임
    
<details> <summary><b>▶ 1️⃣ 충돌 체크 </b></summary>
    
    public class MiniGameCharacterTrigger : MonoBehaviour
    {
        private MiniGameObjectPool gameObjectPool;
    
        protected void OnTriggerEnter(Collider c)
        {
            // 충돌이벤트 설정
            if (!MiniGameManager.Instance.isTriggerEnter)
                return;

            if (c.gameObject.layer == k_CoinsLayerIndex)
            {
                gameObjectPool = c.GetComponent<MiniGameObjectPool>();

                if (gameObjectPool == null)
                {
                    CDebug.Log($"[MINIGAME] OnTriggerEnter null {c.gameObject}");
                    return;
                }

                // 피버모드
                if (MiniGameManager.Instance.IsFeverMode)
                    OnTriggerFeverMode(c);
                else// 일반모드
                    OnTriggerNormalMode(c);
            }
        }
    
        private void OnTriggerNormalMode(Collider c) // 일반 모드에 따른 오브젝트 충돌 처리
        {
            // 아이템 획득 처리
            MiniGameManager.Instance.SetMiniGameItem(gameObjectPool.obj_id);

            switch (gameObjectPool.obj_type)
            {
                case MiniGameObjectPool.MINI_GAME_OBJ_TYPE.RACING_OBJ_HURDLE:
                case MiniGameObjectPool.MINI_GAME_OBJ_TYPE.RACING_OBJ_MONSTER:
                    // 중복 히트 방지
                    StartCoroutine(OnTriggerBoxColliderEnabled());
                    // 무적 모드
                    if (MiniGameManager.Instance.IsInvincibility == false)
                    {
                        SetHitObject(gameObjectPool.obj_type, c);
                    }
                    break;
                case MiniGameObjectPool.MINI_GAME_OBJ_TYPE.RACING_OBJ_COIN:
                case MiniGameObjectPool.MINI_GAME_OBJ_TYPE.RACING_OBJ_TIME:
                case MiniGameObjectPool.MINI_GAME_OBJ_TYPE.RACING_OBJ_BOOST:
                    MiniGameObjectPool.objectPool.Free(c.gameObject);
                    break;
            }
        }
    
        private void OnTriggerFeverMode(Collider c) // 피버모드 충돌 처리
        {
            // 아이템 획득 처리
            MiniGameManager.Instance.SetFeverModeMiniGameItem(gameObjectPool.obj_type);

            switch (gameObjectPool.obj_type)
            {
                case MiniGameObjectPool.MINI_GAME_OBJ_TYPE.RACING_OBJ_HURDLE:
                case MiniGameObjectPool.MINI_GAME_OBJ_TYPE.RACING_OBJ_MONSTER:
                    CSoundControl.Instance?.EffectPlay(SOUND_ID.SFX_MG_BUMP_03);
                    StartCoroutine(gameObjectPool.SetFeverMode());
                    break;

                case MiniGameObjectPool.MINI_GAME_OBJ_TYPE.RACING_OBJ_COIN:
                case MiniGameObjectPool.MINI_GAME_OBJ_TYPE.RACING_OBJ_TIME:
                case MiniGameObjectPool.MINI_GAME_OBJ_TYPE.RACING_OBJ_BOOST:
                    MiniGameObjectPool.objectPool.Free(c.gameObject);
                    break;
            }
        }
    }
        
</details>

#### 🍉 수박게임
    
<details> <summary><b>▶ 1️⃣ 구슬  </b></summary>
  
    public partial class MiniGameWatermelon : MiniGameCommon
    {
        public enum WATERMELON_TYPE
        {
            NONE            = 0,
            BEAD_NORMAL     = 1,
            BEAD_STACKUP    = 2,
            BEAD_RAINBOW    = 3,
        }
    
        private void CreateObjectPool() // 구슬 오브젝트 풀 생성
        {
            if (WatermelonObj == null)
                WatermelonObj = CResourceManager.Instance.LoadObject(CPREFAB_KEY.minigame_watermlon_bead) as GameObject;

            if (WatermelonObj != null)
            {
                MiniGameWatermelonController bead = WatermelonObj.GetComponent<MiniGameWatermelonController>();

                if (bead != null)
                    bead.LoadBeadResource();

                // 구슬 오브젝트 풀링
                objectPool = new MiniGamePooler(WatermelonObj, MiniGamePoolerMax);
            }
        }
    
        public void CreateOBJ(bool isInit = false) / 구슬 생성
        {
            if (GameState == MiniGameState.GameEnd || isTurnEnd || isDeadLine)
                return;

            if (CheckBeadCount())
                return;

            nextBead = objectPool.Get(objPivot.transform.position, Quaternion.identity);

            if (nextBead != null)
            {
                nextBead.transform.SetParent(objPivot.transform, true);
                BeadController = nextBead.GetComponent<MiniGameWatermelonController>();
                var beadType = (WATERMELON_TYPE)BeadListData[bead_rate_value].bead_type;

                BeadController.SetBeadData(beadType, bead_rate_value, BeadListData[bead_rate_value].bead_id, avatarHolder.transform);

                if (isInit)
                {
                    SetNextBeadSprite();
                    UseStartItem();
                    IsBeadDrop = true;
                }

                PageMinigameWmhud.Instance.SetNextBead(); // 다음 구슬 셋팅
                GetMiniGamePlayer.PlayerAnimation.SetGripAnim(); // 캐릭터 애니메이션 셋팅
            }
          }
      }
    
</details>
    
<details> <summary><b>▶ 2️⃣ 구슬 드랍 </b></summary>    
    
    public class MiniGameWatermelonPlayer : MiniGamePlayerInput
    {
        private void ObjectDrop()
        {
            playerAnimation.PlayAnim("isDrop");
            MiniGameWatermelon.Instance.IsBeadDrop = false;

            if (mBeadController != null)
            {
                mBeadController.transform.SetParent(MiniGameWatermelon.Instance.watermelonMap.transform, true);
                mBeadController.ChangeBeadData(MiniGameWatermelon.Instance.map_id);

                MiniGameWatermelon.Instance.AddMap(mBeadController);
                MiniGameWatermelon.Instance.SetMyTurn();
            }

            lineRenderer.positionCount = 0; // 라인 비활성화

            if (nextBeadCo != null)
            {
                StopCoroutine(nextBeadCo);
                nextBeadCo = null;
            }

            nextBeadCo = StartCoroutine(MiniGameWatermelon.Instance.CreateNextOBJ());
            SetisPlayerHit(false);
        }
    }
    
</details>
    
<details> <summary><b>▶ 3️⃣ 충돌 처리 </b></summary>     
    
    private void OnHit(Collision2D collision)
    {
        if (collision.gameObject == null || gameObject.activeInHierarchy == false)
            return;

        if (isHit)
            return;
        else
        {
            // 바닥에 부딪혔을 때
            if(isTriggerEnter == false)
            {
                if (collision.gameObject.CompareTag("obj_resultcard"))
                {
                    isTriggerEnter = true;
                    ResetIsFly();
                    MiniGameWatermelon.Instance.SetPlayerMove(true);
                    return;
                }
            }
        }

        // 타겟 충돌
        if (collision.gameObject.TryGetComponent<MiniGameWatermelonController>(out var targetController))
        {
            switch (m_beadType)
            {
                // 구슬 노멀
                case MiniGameWatermelon.WATERMELON_TYPE.BEAD_NORMAL:
                    //같은 단계의 구슬
                    if (targetController.m_beadStep == m_beadStep)
                    {
                        if (targetController.m_beadType != MiniGameWatermelon.WATERMELON_TYPE.BEAD_RAINBOW)
                            SetHit(targetController);
                    }
                    else
                    {
                        CheckMoving();
                        // 다른 단계의 구슬(이동만 가능하게)
                        MiniGameWatermelon.Instance.SetPlayerMove(true);
                    }

                    break;
                // 안터지는 구슬
                case MiniGameWatermelon.WATERMELON_TYPE.BEAD_STACKUP:
                    isTriggerEnter = true;
                    CheckMoving();
                    MiniGameWatermelon.Instance.SetPlayerMove(true);
                    break;
                // 모든 종류의 TYPE_NORMALBEAD 구슬과 합쳐짐
                case MiniGameWatermelon.WATERMELON_TYPE.BEAD_RAINBOW:
                    // 안터지는 구슬 (무지개) 제외 모든 구슬
                    if (targetController.m_beadType != MiniGameWatermelon.WATERMELON_TYPE.BEAD_STACKUP && targetController.m_beadType != MiniGameWatermelon.WATERMELON_TYPE.BEAD_RAINBOW)
                    {
                        SetHit(targetController, true);
                    }
                    break;
            }
        }
    }
        
</details>
<br />
  

#### 4️⃣ **프로필 및 프레임 시스템**
유저의 개성을 표현하는 프로필 사진과 프레임 선택 시스템을 구축하고, 획득 조건에 따른 활성화 로직을 구현.
- **주요 구현 :**
    - 수백 종류의 프로필 리소스를 테이블 데이터와 연동하여 코드 수정 없이 리소스를 추가/관리할 수 있는 구조 적용.
    - **상태별 필터링 로직 :** '보유 중', '미보유', '장착 중' 상태를 구분하고, 미보유 아이템의 경우 획득 처 확인 가능 팝업 연동.
	- 대량 프로필 이미지의 메모리 사용 최적화를 위해 SuperScrollView의 Cell 재사용과 Addressables 기반 동적 리소스 로딩 적용

<p align="left">
  <img src="https://github.com/user-attachments/assets/556d6878-9cf8-4d87-9aa4-21a8ab4f8f6c" width="30%" alt="Profile Image">
  <img src="https://github.com/user-attachments/assets/585c73e8-38c6-42f3-af74-37c34da95ca1" width="30%" alt="Profile Frame">
</p>
<br />

#### 5️⃣ **스페셜 카드 시스템**
오프라인 실물 카드의 QR 코드를 통해 게임 내 카드를 등록 및 해제할 수 있는 시스템 구현.
- **주요 구현 :**
    - 카드별 고유 QR 코드 스캔을 통한 1인 1카드 등록 및 해제 처리.
    - 테이블 데이터와 서버 API를 통해 등록 여부 검증을 중복 등록을 방지.
    - 카드의 등록 및 해제/재등록 코드 이력을 확인할 수 있는 히스토리 구현.

<p align="left">
  <img src="https://github.com/user-attachments/assets/e1de66e6-9aae-4943-b595-c4685ae9b04f" width="30%" alt="Special Card Main">
  <img src="https://github.com/user-attachments/assets/793ae346-b90f-43ed-9166-9f4440924446" width="30%" alt="Special Card QR">
</p>

- **개선/최적화:** QR 리더기의 인식 이벤트가 팝업 생성 및 처리 완료 전에 연속적으로 호출됨. 동일한 등록 요청이 여러 번 실행되어 등록 팝업이 중복으로 생성되는 문제가 발생. => 팝업 활성 상태 체크를 통해 중복 생성 방지 및 Coroutine을 활용해 QR Reader실행 타이밍 제어로 해결.

### 💻 Code Snippets    
<details> <summary><b>▶ QR코드 인식 </b></summary>    

    protected override void OnStart()
	  {
		base.OnStart();

		SetType(UIPOPUP_TYPE.PAGE_POPUP);

		RectTransform ret = GetComponent<RectTransform>();
		ret.offsetMin = Vector2.zero;
		ret.offsetMax = Vector2.zero;

        if (tipCamera != null)
            tipCamera.text = "";

        StartReader();

        if (descText != null)
            descText.text = CStringDataManager.Instance.GetStringData("AR_system_tip_5");

        SetTipText();
    }

	protected override void OnDestroy()
	{
		base.OnDestroy();

        StopReader();
    }

    private void StartReader()
	{
		if (reader != null)
		{
			reader.StartWork();
            CodeReader.OnCodeFinished += CheckCardRegister;
		}
	}

	private void StopReader()
	{
        if (reader != null)
        {
            reader.StopWork();
            CodeReader.OnCodeFinished -= CheckCardRegister;
        }
    }

    // 이미 본 계정에 등록되어있는지 체크
    private void CheckCardRegister(string codeData)
    {
        // codeData = QR코드 일련번호
        cardList = SpCardManager.Instance.GetSpCardList();
        SetTipText();

        if (cardList != null)
        {
            if (cardList.Find(x => x.spcd_code == codeData) != null) // 이미 본 계정에 등록된 카드
            {
                SetTipText("AR_system_error_3");
                return;
            }
            else
                OpenPopupSpcardSetting(codeData);
        }
        else
            OpenPopupSpcardSetting(codeData);
    }
    private void SetTipText(string text = null)
	  {
        tipObj.SetActive(text != null);
        if (tipText != null && text != null)
            tipText.text = CStringDataManager.Instance.GetStringData(text);
    }

    private void SetCameraTip(string text = null)
    {
        tipObj.SetActive(text != null);
        if (tipText != null && text != null)
            tipText.text = CStringDataManager.Instance.GetStringData(text);
    }

    /// <summary>
    /// 아직 업데이트 전 카드
    /// </summary>
    /// <param name="card_id"></param>
    private bool CheckUpdateCard(long card_id)
    {
        bool isUpdate = false;

        var cardData = CCardDataManager.Instance.GetCardListData(card_id);

        if(cardData != null)
        {
            if (cardData.card_useable == COMMON_AVAILABILLITY.BOOL_DISABLE)
            {
                CCoreServices.GetCoreService<CPopupService>().NoticeMessageDisposable("AR_system_error_10");
                SetTipText("AR_system_error_10");
                isUpdate = true;
            }
        }

        return isUpdate;
    }

    private void OpenPopupSpcardSetting(string codeData)
	  {
		if (reader.IsWorking)
            reader.StopWork();

		if (CCoreServices.GetCoreService<CPopupService>().IsOpenPopup<PopupSpcardSetting>()) // 동일한 팝업 있는지 체크
			return;

        var checkDispose = new SingleAssignmentDisposable();
        checkDispose.Disposable = APIHelper.SpecialCardService.Req_SpecialcardCheck(codeData)
            .SelectMany(res =>
            {
                if (res != null)
                {
                    // 사용 가능 카드
                    if (res.d.is_valid)
                    {
                        var popup = new SingleAssignmentDisposable();
                        popup.Disposable = CCoreServices.GetCoreService<CPopupService>().OpenPopupSpcardSetting(codeData, -1, cardList, this)
                            .ShowAsObservable().Do(_ => popup.Dispose()).Subscribe();
                    }
                    else
                    {
                        if (res.d.card_id > 0)
                        {
                            //업데이트 카드 체크
                            CheckUpdateCard(res.d.card_id);
                        }
                        else
                        {
                            SetTipText("AR_system_error_4");
                        }

                        StartCoroutine(StartWork());
                    }
                }
                return Observable.ReturnUnit();
            }).Do(_ => checkDispose.Dispose()).Subscribe();
	}

    protected IEnumerator StartWork()
    {
        yield return new WaitForSeconds(0.6f);
        reader.StartWork();
    }
  
</details>  
<br />

#### 6️⃣ **컨텐츠 해금**
플레이 중 특정 해금 조건을 달성하면 콘텐츠가 해금되는 시스템을 구현.
- **주요 구현 :**
    - 컨텐츠별 해금 조건 및 해금 상태 관리 및 조건 달성 시 해금 팝업 노출.
    - 해금 팝업의 이동 버튼을 통한 해당 컨텐츠로의 네비게이션 이동.
    - 미해금 컨텐츠 클릭 시 해금 조건을 안내하는 알림 팝업 노출.

- **개선/최적화:** 로비 씬 이동 전에 컨텐츠 해금 여부에 필요한 서버 데이터 셋팅이 완료되지 않아 해금 상태가 정상적으로 표시되지 않는 문제가 발생. 모든 데이터 처리가 완료되는 정확한 시점을 판단하기 어려웠음. => 서버 데이터 초기화 과정을 분석하여 각 데이터 셋팅 및 후속 처리가 완료된 시점에 콜백을 호출해 씬이동 구성.
<br />

   
---

## 📈 Growth & Review
- **데이터 설계 능력 향상 :** Normal/Hard/Event 등 복잡한 라이브 서비스 환경에서 데이터 간섭 없이 안정적으로 확장 가능한 시스템 설계 역량을 쌓았습니다.
- **최적화 고려 :** 대량의 랭킹 데이터 처리 및 오브젝트 풀링을 활용한 미니게임 구현 등을 통해 성능 최적화의 중요성을 체득했습니다.
- **협업 중심 개발 :** 기획 의도에 유연하게 대응할 수 있도록 추상화와 인터페이스 중심의 코드를 작성하여 유지보수 효율을 높였습니다.

---
## 🎮 Project 2: Project V (개발 중단/미출시)
> **몰입감 있는 스토리 안에서 캐릭터를 수집하고, 생성형 AI로 세계가 확장되는 하이브리드 게임의 핵심 시스템 개발**
    
<p align="left">
  <img src="https://github.com/user-attachments/assets/a4092c9f-b1d0-409a-a1e0-e57f2896c7c7" width="30%" alt="PJV Start">
  <img src="https://github.com/user-attachments/assets/c6e925fb-3f6d-47cc-99a8-67e1fe6f150e" width="30%" alt="PJV Main">
</p>

## 📌 Project Overview
- **개발 기간 :** 2025.10 ~ 2026.02
- **기술 스택 :** Unity 3D, C#, UGUI, UniRx
- **담당 역할 :** 클라이언트 컨텐츠 로직 구현 (SNS)
<br />
    
### ✨ Key Implementation

### **Mobile SNS System (Instagram)**
> **사용자 간 피드 공유 및 실시간 상호작용이 가능한 모바일 SNS 플랫폼 구현**
    
#### 1️⃣ **피드 업로드 및 실시간 댓글 시스템 📤**
유저가 직접 미디어를 공유하고 댓글로 상호작용할 수 있는 커뮤니티 핵심 기능을 구현

- **주요 구현 :** 
  - **미디어 업로드 로직 :** 테이블 데이터 기반하여 이미지를 로드하고 이미지 선택과 함께 글을 작성하여 서버 전송 적용
  - **실시간 댓글 동기화 :** 댓글 작성 시 리스트 갱신 및 레이아웃을 동적으로 높이 조절 처리
  - 좋아요 및 댓글 등록 시 UI를 즉시 반영하여 사용자 체감 속도 극대화
    
<p align="left">
  <img src="https://github.com/user-attachments/assets/38149c0b-08e0-4b14-a8be-90d23d5bf0ab" width="30%" alt="Feed Main">
  <img src="https://github.com/user-attachments/assets/7867e977-a019-4b54-a51d-77bea008fe8a" width="30%" alt="Feed Replying">
  <img src="https://github.com/user-attachments/assets/fd29a11b-a10d-462e-8f87-a13e827b88e6" width="30%" alt="Feed Upload">
</p>
  
- **개선/최적화:** 
    - 피드 콘텐츠의 높이가 가변적임에 따라, 각 게시물의 레이아웃이 고정되지 않아 컴포넌트 간 중첩 현상이 발생. 이를 해결하기 위해, 각 피드를 구성하는 하위 요소들의 높이를 실시간으로 연산하고, 가변적인 댓글 영역의 높이까지 합산하여 전체 피드의 레이아웃 사이즈를 동적으로 재설정함으로써 레이아웃 중첩 문제를 해결.
 
 ### 💻 Code Snippets
 <details> <summary><b>▶ 피드 레이아웃 재조정</b></summary>
  
    private void RefreshRootUIHeight()
    {
        Canvas.ForceUpdateCanvases();

        float childrenHeight = 0f;
        foreach (var child in rectList)
        {
            childrenHeight += child.rect.height;
        }
        childrenHeight += replySpacing; // 댓글 제외한 전체 높이 + 보정값

        replyParentRect.anchoredPosition = new Vector2(replyParentRect.anchoredPosition.x, childrenHeight * -1); // 댓글 위치 조정

        var replyHeight = feedData.reply_cnt > 0 ? replyParentRect.rect.height : 0;
        var sizeDelta = rootUI.sizeDelta;
        sizeDelta.y = childrenHeight + replyHeight; // 전체 사이즈 조정
        rootUI.sizeDelta = sizeDelta;
    }
  
</details>
 <br />   

#### 2️⃣ **실시간 소셜 상호작용 ❤️**
좋아요, 댓글 등 유저 간 실시간 인터랙션을 위한 반응형 UI 시스템을 개발

- **주요 구현 :** 
  - 유저 액션에 따른 게시물의 상태 변경(좋아요 수 , 댓글 등)이 반영되도록 적용
    
<p align="left">
  <img src="https://github.com/user-attachments/assets/00949b2f-d98f-4af2-85b7-95d4aab56113" width="30%" alt="Feed Like">
  <img src="https://github.com/user-attachments/assets/90901293-3db1-46ce-8b0a-78bece3d1c6f" width="30%" alt="Story Like">
</p>
<br />

#### 3️⃣ 스토리 시스템 ⏱️
24시간 동안 유지되는 휘발성 콘텐츠를 위한 별도의 뷰어와 업로드 로직을 구현

- **주요 구현 :** 
  - **스토리 전용 뷰어 :** 상단 프로그레스 바 연동 및 자동 넘김, 탭 이동 기능 구현
  - **필터링 로직 :** 서버 데이터 기반으로 남은 유효시간을 계산해 실시간 만료 카운트다운을 수행하여, 유효 시간이 경과한 데이터는 자동으로 필터링해 노출되지 않도록 구현
  - **프로필 프레임 상태 변경 :** 스토리 확인 유무에 따른 프로필 프레임 상태 변경 적용
    
<p align="left">
  <img src="https://github.com/user-attachments/assets/90901293-3db1-46ce-8b0a-78bece3d1c6f" width="30%" alt="Story">
</p>

### 💻 Code Snippets
<details> <summary><b>▶ 1️⃣ 스토리 및 상단 프로그레스 바 셋팅</b></summary>
  
    public class PopupSnsStory : CPopupUI<PopupSnsStory.Setting, PopupSnsStory.Result>
    {
         private List<ObjSnsStory> objStoryList = new List<ObjSnsStory>(50); // 재사용을 위한 story obj 리스트
         private List<ObjSnsStorySlider> objSliderList = new List<ObjSnsStorySlider>(50); // 재사용을 위한 slider obj 리스트

         private void SetStoryInfo(bool isNext)
          {
              objStoryList?.Clear();

              storyList = objStoryData.storyList;

              for (int i = 0; i < storyList.Count; i++)
              {
                  var story = storyList[i];
                  var prefabData = CSNSDataManager.Instance.GetStoryPrefabData(story.prefab_id);
                  if (prefabData != null)
                  {
                      var storyObj = Instantiate(templateObj[prefabData.storyprfb_type], storyHolder);
                      if(storyObj != null)
                      {
                          var objSnsStory = storyObj.GetComponent<ObjSnsStory>();
                          if (objSnsStory != null)
                          {
                              objSnsStory.Init(storyList[i], i, this);
                              objStoryList.Add(objSnsStory);
                          }
                      }

                      if(objSliderList.Count < objStoryList.Count) // slider가 story갯수보다 적으면 생성
                      {
                          var slider = Instantiate(sliderObj, sliderHolder);
                          if (slider != null)
                          {
                              slider.gameObject.SetActive(true);
                              var objSnsStorySlider = slider.GetComponent<ObjSnsStorySlider>();
                              if (objSnsStorySlider != null)
                              {
                                  objSnsStorySlider.Init(this);
                                  objSliderList.Add(objSnsStorySlider);
                              }
                          }
                      }
                  }
              }

              if(objStoryList.Count > 0)
              {
                  SetActiveSlider(true); // 스토리갯수만큼 슬라이더 켜기

                  storyIdxMax = storyList.Count - 1;
                  if (isNext)
                  {
                      SetCurStoryIdx(0);
                      SetSliderMin();
                  }
                  else
                  {
                      SetCurStoryIdx(storyIdxMax); // 이전스토리는 맨 마지막 스토리부터 시작
                      SetSliderMax();
                  }    

                  SetSibilingStory(true, true);
              }
          }
    }
    
</details>
  
<details> <summary><b>▶ 2️⃣ 다음 데이터 셋팅 </b></summary>
  
    public class PopupSnsStory : CPopupUI<PopupSnsStory.Setting, PopupSnsStory.Result>
    {
        private bool CheckSibilingData(bool isNext) // 다음 캐릭터의 스토리가 있는지 체크
        {
            int curIdx = index;
            var silibingIdx = isNext ? ++curIdx : --curIdx;

            var objectStoryDataList = APIHelper.SnsService.GetStoryData();
            var siblingData = silibingIdx >= 0 && silibingIdx < objectStoryDataList.Count ? objectStoryDataList[silibingIdx] : null;

            if (siblingData != null)
            {
                objStoryData = siblingData;
                return true;
            }

            return false;
        }

        public void OnClickLeft() // 왼쪽으로 넘기기
        {
            SetCurStoryIdx(--curStoryIdx);
            if(curStoryIdx < 0)
            {
                if (CheckSibilingData(false))
                    SetSibilingObjectData(false);
                else
                {
                    SetCurStoryIdx(0);
                    SetSibilingStory(false);
                }
            }
            else
                SetSibilingStory(false);
        }

        private void SetSibilingStory(bool isRight, bool isInit = false, bool isCheckExpired = false) // 다음 스토리 및 슬라이더 셋팅
        {
            if(!isCheckExpired)
            {
                if (!isInit && storyIdxMax > 0)
                {
                    var preIdx = isRight ? curStoryIdx - 1 : curStoryIdx + 1;

                    objStoryList[preIdx].gameObject.SetActive(false);
                    objSliderList[preIdx].SetTimeSliderValue(isRight);
                }
            }

            objStoryList[curStoryIdx].gameObject.SetActive(true);
            objStoryList[curStoryIdx].SetStoryInfo();

            objSliderList[curStoryIdx].StartTimeSlider();
        }
    }
  
</details>
  
<details> <summary><b>▶ 3️⃣ 시간 만료 체크 </b></summary>

    public class PopupSnsStory : CPopupUI<PopupSnsStory.Setting, PopupSnsStory.Result>
    {                                                                             
        public void RemoveExpiredStory() // 시간 만료된 스토리 삭제
        {
            isDataDeleted = true;

            SetReadCheck(curStoryData.gdid, false); 
            objStoryData.storyList.RemoveAt(curStoryIdx);
            objStoryList[curStoryIdx].gameObject.SetActive(false);
            objSliderList.FindLast(x => x.gameObject.activeSelf).gameObject.SetActive(false); // 활성화O && 맨끝에 있는 슬라이더를 꺼야함

            var newObjStoryList = new List<ObjSnsStory>(); // 스토리오브젝트 재구성
            for (int i = 0; i < objStoryList.Count; ++i)
            {
                if (i != curStoryIdx)
                    newObjStoryList.Add(objStoryList[i]);
                else
                    objStoryList[i].gameObject.Destroy();
            }

            storyList = objStoryData.storyList;
            objStoryList = newObjStoryList;

            storyIdxMax = storyList.Count - 1;

            if(storyIdxMax < 0)
            {
                APIHelper.SnsService.CheckStoryListNone(); // 스토리 남았는지 체크 후, 데이터 갱신
                SNSManager.Instance.PageSNS.SetStory();
                --index;
            }

            for (int i = 0; i < newObjStoryList.Count; ++i)
            {
                objStoryList[i].RefreshData(storyList[i], i);
            }

            SetCurStoryIdx(curStoryIdx);
            if(curStoryIdx > storyIdxMax) // 왼 -> 오
            {
                if (CheckSibilingData(true))
                    SetSibilingObjectData(true);
                else
                {
                    CCoreServices.GetCoreService<CPopupService>().NoticeMessageDisposable("STORY_toast_nostorytoplay");
                    OnClickClose();
                }
                return;
            }

            SetSibilingStory(true, false, true);

            isDataDeleted = false;
            isTestCompleted = false;
        }
    }

  </details>

<br />

#### 4️⃣ **실시간 DM 시스템 💬**
유저와 캐릭터 간 1:1 채팅을 위한 메시지 송수신 시스템을 구축
- **주요 구현 :** 
  - **실시간 통신 :** 서버- AI간 데이터를 실시간 이벤트 기반으로 중계하여 대화 내용이 즉시 화면에 반영되는 대화형 인터페이스 구축
  - **채팅 UI 최적화 :** 가변형 말풍선 리스트 구현
  - **상태 표시 :** 지연 메세지, 메시지 읽음 처리 및 실시간 글쓰기 상태 표시 로직 연동

<p align="left">
  <img src="https://github.com/user-attachments/assets/3cc73722-43fe-4b52-add9-213e8343902f" width="30%" alt="DM List">
  <img src="https://github.com/user-attachments/assets/156a80a0-af78-435f-9c93-48a12cc8e21d" width="30%" alt="DM">
</p>

---
 
> **"복잡한 로직을 단순하고 견고하게 설계하는 것을 즐깁니다. 유저에게 즐거운 경험을 주기 위해 끊임없이 고민하는 클라이언트 개발자가 되겠습니다."**
