# 📂 Project Portfolio | Client Develop
> **Unity 클라이언트 개발자 [김민선]입니다.**

## 📋 Table of Contents
1. [NCT ZONE - 콘텐츠 및 시스템 개발](#-project-1-nct-zone)
2. [Project V](#-project-2-project-v)

---

## 🎮 Project 1: NCT ZONE
> **NCT 멤버들과 함께하는 시네마틱 어드벤처 게임의 핵심 시스템 개발 및 라이브 서비스 개발/유지 보수**

<p align="left">
  <img src="https://github.com/user-attachments/assets/3fffa844-a235-4d50-b18a-db2b56d28fde" width="20%" alt="Icon">
  <img src="https://github.com/user-attachments/assets/03074ce6-78da-4dc3-8843-da0f0fab0dd7" width="20%" alt="NCT ZONE Main">
</p>

### 📌 Project Overview
- **개발 기간 :** 2022.11 ~ 2026.02
- **기술 스택 :** Unity 3D, C#, UGUI
- **담당 역할 :** 클라이언트 컨텐츠 로직 구현 (스테이지, 시스템, 미니게임, 덱, 빙고게임)

### ✨ Key Implementation

#### 1️⃣ 스테이지 & 챕터 시스템 (Stage & Chapter)
다양한 게임 모드(Normal, Hard, Event)에 대응하기 위해 확장성 있는 데이터 구조를 설계하고 관리 시스템을 구현했습니다.
- **주요 구현 :** 
    - 챕터 간 이동 및 해금 조건 체크 시스템 구현
    - **모드별 데이터 격리 구조 :** Enum 타입을 활용한 스테이지 카테고리 분류 및 모드별 독립적 데이터 테이블 관리로 데이터 혼선 방지.
    - **이벤트 스테이지 제어 :** 기간 한정 컨텐츠를 위해 서버 시간과 연동된 활성화/비활성화 스케줄링 로직 구현.
    - **추상화 기반 설계 :** 각 모드가 동일한 인터페이스를 공유하되, 보상 산정 및 난이도 계산 로직은 각 모드에 맞게 동작하도록 설계.

<p align="left">
  <img src="https://github.com/user-attachments/assets/6006a790-1e9b-4d6b-b1ee-1a1308b93348" width="20%" alt="Stage System">
  <img src="https://github.com/user-attachments/assets/71e51b5b-0d2b-477a-abeb-23ee61b53412" width="20%" alt="Worldmap">
  <img src="https://github.com/user-attachments/assets/484f0407-ebd6-4300-84f3-f0b2626e0b33" width="20%" alt="Stage Enter Popup">
</p>
 
- **Troubleshooting:** 
- 스테이지 전환 시의 메모리 부하를 줄이기 위해 어드레서블(Addressables)을 활용한 리소스 로드 최적화
- 기존 시스템에서 하드 모드와 이벤트 스테이지가 추가됨에 따라 데이터 구조가 복잡해지는 문제가 발생.
이를 해결하기 위해 각 모드를 Data Layer에서 분리하고, partial 키워드를 통해 class를 나누고 데이터 오염 방지와 유지보수 편의성을 동시에 확보.

#### 2️⃣ 덱 저장 시스템 (Deck Management)
유저가 스테이지 공략을 위해 설정한 최적의 카드 조합을 로컬 및 서버에 저장하고 불러오는 기능을 구현했습니다.
- **주요 구현 :**
    - 다중 덱 슬롯 관리 로직
    - 보스별 속성 상성과 카드의 상성에 따른 빠른 편성 기능 추가 

<p align="left">
  <img src="https://github.com/user-attachments/assets/f0d3182c-3baf-4f0c-ac87-f5ed9096adc7" width="20%" alt="Deck Editing1">
  <img src="https://github.com/user-attachments/assets/0c3e2702-a99c-4fd0-af3a-9c289ea0b692" width="20%" alt="Deck Editing2">
</p>

- **Troubleshooting:** 기존 시스템에서 타입별로 덱을 구성하게됨에 따라 데이터 구조가 복잡해지는 문제가 발생.
이를 해결하기 위해 상속을 통해 공통 로직은 재사용하되 타입별 특수 로직은 개별적으로 처리하도록 구현하여 데이터 오염 방지 및 유지 보수 편의성을 동시에 확보.

#### 3️⃣ 빙고 이벤트 시스템 (Bingo System)
미션 수행과 재화 소모를 결합하여 유저의 리텐션을 유도하는 이벤트 시스템을 개발했습니다.
- **주요 구현 :**
    - **이원화 도장 시스템 :** 미션 보상(일반 도장)과 유료 재화(스페셜 도장) 사용 로직 구현
    - **빙고 판정 알고리즘 :** 5x5 그리드의 가로, 세로, 대각선 완성 여부를 실시간 검출
    - **최종 보상 연동 :** 올빙고 달성 시 '미공개 사진' 뷰어 및 해금 연출 구현

<p align="left">
  <img src="https://github.com/user-attachments/assets/b81fe47f-739d-4c25-977c-e8ad123d71a8" width="20%" alt="Bingo">
  <img src="https://github.com/user-attachments/assets/6e088cdf-e081-4584-b003-817c277f46e8" width="20%" alt="Bingo Line Clear">
  <img src="https://github.com/user-attachments/assets/e3c224e0-ba80-4e0d-968b-149e660786eb" width="20%" alt="Bingo Mission">
</p>

#### 3️⃣ 미니게임 2종 (Mini Games)
게임 내 체류 시간을 높이기 위한 캐주얼 미니게임의 핵심 메카닉을 개발했습니다.

##### 🏎️ 레이싱 미니게임
- 물리 기반의 차량 컨트롤러 구현
- 무한 맵 생성을 위한 오브젝트 풀링(Object Pooling) 적용

<p align="left">
  <img src="https://github.com/user-attachments/assets/60bed8a2-d169-4c27-b874-ecfac5a0cd05" width="20%" alt="Racing Main">
  <img src="https://github.com/user-attachments/assets/62c30b50-00df-4cc7-8a7e-3ad5d9c26121" width="20%" alt="Racing Playing">
</p>

- **Troubleshooting:** 다양한 아이템 연출이 겹칠 때 프레임 드랍이 발생하는 문제를 방지하기 위해, 이펙트 오브젝트에도 오브젝트 풀링을 적용하여 안정적인 성능을 유지.

##### 🍉 수박게임 (Merge Mechanic)
- 오브젝트 풀링을 이용한 게임 최적화
- 원형 물리 충돌 판정을 이용한 오브젝트 합성 로직
- 상단 투하 위치 가이드 및 스코어 시스템

<p align="left">
  <img src="https://github.com/user-attachments/assets/8d2f26f2-0c23-4760-affc-bdfa4dbefb5c" width="20%" alt="Watermelon Main">
  <img src="https://github.com/user-attachments/assets/0bcb49e5-613d-43d8-826c-598bda604a12" width="20%" alt="Watermelon Playing">
  <img src="https://github.com/user-attachments/assets/a8f720b0-78f1-48f0-a938-a6ddbb768732" width="20%" alt="Watermelon Select Item">
</p>

##### 미니게임 공통 적용 사항
- **아이템 시스템 :** 각기 다른 효과(턴 추가, 통흔들기, 지정해서 없애기 등)를 가진 아이템을 고유 트리거 시스템을 통해 사용 즉시 연동되는 특수 효과 및 로직을 구현하고, 게임의 템포를 조절하는 다양한 변수 창출.
- **랭킹 시스템 :** 전체 유저 대상의 '글로벌 랭킹'과 소셜 연동 정보를 활용한 '친구 랭킹' 데이터 분리 및 UI 대응.

<p align="left">
  <img src="https://github.com/user-attachments/assets/8b2d59ba-cf36-4f75-9f5d-71909796d7fa" width="20%" alt="MiniGame Ranking System">
</p>

- **Troubleshooting:** 다양한 아이템 연출이 겹칠 때 프레임 드랍이 발생하는 문제를 방지하기 위해, 이펙트 오브젝트에도 오브젝트 풀링을 적용하여 안정적인 성능을 유지.

#### 4️⃣ 프로필 및 프레임 시스템 (Profile & Frame System)
유저의 개성을 표현하는 프로필 사진과 프레임 선택 시스템을 구축하고, 획득 조건에 따른 활성화 로직을 구현했습니다.
- **주요 구현 :**
    - 수백 종류의 프로필 리소스를 시트 데이터와 연동하여 코드 수정 없이 리소스를 추가/관리할 수 있는 구조 설계.
    - **상태별 필터링 로직 :** '보유 중', '미보유', '장착 중' 상태를 구분하고, 미보유 아이템의 경우 획득 처 확인 가능 팝업 연동.
    - **동적 리소스 로딩 :** 메모리 효율을 위해 모든 프레임을 상주시키지 않고, 필요한 시점에만 Addressables 또는 Resources를 통해 로드하는 최적화 적용.

<p align="left">
  <img src="https://github.com/user-attachments/assets/cd324a3e-1f37-4776-8cfe-ad08097f4178" width="20%" alt="Profile Image">
  <img src="https://github.com/user-attachments/assets/007dcec3-47ee-4a71-9f11-576af00d73dc" width="20%" alt="Profile Frame">
</p>


## 💻 Code Snippets

### [Stage & Chapter System]

<details>
<summary><b>1️⃣ 모드별 데이터 구분을 위한 구조</b></summary>
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


### [Deck System]

<details>
<summary><b>1️⃣ 덱 데이터 구조 및 저장 로직 (클릭하여 펼치기)</b></summary>
   
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

<details> 
<summary><b>2️⃣ 세부 덱 정보 (클릭하여 펼치기)</b></summary>

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
    
### [Bingo Game]

<details>
<summary><b>1️⃣ 슬롯 데이터 </b></summary>
    
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
    
    public class PageBingoEvent : PagePopup
    {
        private BingoInfo bingoEventData; // 서버 데이터
        private List<BingoList> bingoStepList = new List<BingoList>(); // 테이블 데이터
    
        private void Init()
        {
            bingoEventData = APIHelper.BingoService.BingoInfo;
            bingoStepList = APIHelper.BingoService.BingoStepList;
            curEventTbData = CEventDataManager.Instance.GetBingoEventData(bingoEventData.gdid);

            SetCurBingoStep(APIHelper.BingoService.GetLastOpenBingoData());
            SetLineRwdDic();
            SetEventInfo();
            SetStampInfo();
            SetEventTimer();
            SetMissionRedDot(bingoEventData.mission_reddot);
        }
    
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
    }

</details>
    
<details> <summary><b>1️⃣ 라인 완성 체크 및 연출 </b></summary>
    
    public class ObjBingoSlot : MonoBehaviour
    {
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

### [Mini Game]
#### 🏎️ 레이싱 미니게임
    
<details> <summary><b>1️⃣ 충돌 체크 </b></summary>
    
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
    
<details> <summary><b>1️⃣ 구슬  </b></summary>
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
    
<details> <summary><b>2️⃣ 구슬 드랍 </b></summary>    
    
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
    
<details> <summary><b>3️⃣ 충돌 처리 </b></summary>     
    
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
                        //CheckFly();
                        CheckMoving();
                        // 다른 단계의 구슬(이동만 가능하게)
                        MiniGameWatermelon.Instance.SetPlayerMove(true);
                    }

                    break;
                // 안터지는 구슬
                case MiniGameWatermelon.WATERMELON_TYPE.BEAD_STACKUP:
                    isTriggerEnter = true;
                    //CheckFly();
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
    
---

## 📈 Growth & Review
- **데이터 설계 능력 향상 :** Normal/Hard/Event 등 복잡한 라이브 서비스 환경에서 데이터 간섭 없이 안정적으로 확장 가능한 시스템 설계 역량을 쌓았습니다.
- **최적화 및 UX 고려 :** 대량의 랭킹 데이터 처리 및 오브젝트 풀링을 활용한 미니게임 구현을 통해 성능 최적화와 유저 피드백(연출)의 중요성을 체득했습니다.
- **협업 중심 개발 :** 기획 의도에 유연하게 대응할 수 있도록 추상화와 인터페이스 중심의 코드를 작성하여 유지보수 효율을 높였습니다.

---
## 🎮 Project 2: Project V 
> **몰입감 있는 스토리 안에서 캐릭터를 수집하고, 생성형 AI로 세계가 확장되는 하이브리드 게임의 핵심 시스템 개발**
    
<p align="left">
  <img src="https://github.com/user-attachments/assets/b14ec014-eeda-4263-9d3b-d00410f524ed" width="20%" alt="PJV Start">
  <img src="https://github.com/user-attachments/assets/a69c446b-6528-4087-854e-9cf24aa575d7" width="20%" alt="PJV Main">
</p>

## 📌 Project Overview
- **개발 기간 :** 2025.10 ~ 2026.02
- **기술 스택 :** Unity 3D, C#, UGUI
- **담당 역할 :** 클라이언트 컨텐츠 로직 구현 (SNS)
    
### ✨ Key Implementation

### Mobile SNS System (Instagram Clone)
> **사용자 간 피드 공유 및 실시간 상호작용이 가능한 모바일 SNS 플랫폼 구현**
    
#### 1️⃣ 피드 업로드 및 실시간 댓글 시스템 (Feed & Comment) 📤
유저가 직접 미디어를 공유하고 의견을 나눌 수 있는 커뮤니티 핵심 기능을 구현

- **주요 구현 :** 
  - **미디어 업로드 로직 :** 테이블 데이터 기반하여 이미지를 로드하고 이미지 선택과 함께 글을 작성하여 서버 전송 적용
  - **실시간 댓글 동기화 :** 댓글 작성 시 리스트 자동 갱신 및 레이아웃 동적으로 높이 조절(Dynamic Layout) 처리
  - 좋아요 및 댓글 등록 시 서버 응답 전 UI를 즉시 반영하여 사용자 체감 속도 극대화
    
<p align="left">
  <img src="https://github.com/user-attachments/assets/f91e191f-89fe-471f-aeee-d81b78cc6921" width="20%" alt="Feed Main">
  <img src="https://github.com/user-attachments/assets/5cace5de-6352-4547-a5c2-59b6af57eb59" width="20%" alt="Feed Replying">
  <img src="https://github.com/user-attachments/assets/e21cca3f-3b8f-462a-925d-d07d64c1078e" width="20%" alt="Feed Upload">
</p>
  
- **Troubleshooting:** 
- 피드 콘텐츠의 높이가 가변적임에 따라, 각 게시물의 레이아웃이 고정되지 않아 컴포넌트 간 중첩 현상이 발생. 이를 해결하기 위해, 각 피드를 구성하는 하위 요소들의 높이를 실시간으로 연산하고, 가변적인 댓글 영역의 높이까지 합산하여 전체 피드의 레이아웃 사이즈를 동적으로 재설정함으로써 레이아웃 중첩 문제를 해결.
    
#### 2️⃣ 실시간 소셜 상호작용 (Social Interaction) ❤️
좋아요, 댓글 등 유저 간 실시간 인터랙션을 위한 반응형 UI 시스템을 개발

- **주요 구현 :** 
  - 서버 응답 대기 없이 유저 액션에 즉각 반응하는 UI 로직을 적용하여 체감 속도 향상
  - 게시물의 상태 변경(좋아요 수 등)이 여러 UI 뷰에 동시 반영되도록 적용
    
<p align="left">
  <img src="https://github.com/user-attachments/assets/a3ae732c-a8c6-4108-b21a-d217ff8141c1" width="20%" alt="Feed Like">
  <img src="https://github.com/user-attachments/assets/31d89a78-2bd0-4b56-87a3-7e95dc694450" width="20%" alt="Story Like">
</p>

#### 3️⃣ 스토리 시스템 (Story System) ⏱️
24시간 동안 유지되는 휘발성 콘텐츠를 위한 별도의 뷰어와 업로드 로직을 구현

- **주요 구현 :** 
  - **스토리 전용 뷰어 :** 상단 프로그레스 바(Progress Bar) 연동 및 자동 넘김, 탭 이동 기능 구현
  - **필터링 로직 :** 서버 데이터 기반으로 남은 유효시간을 계산해 실시간 만료 카운트다운을 수행하여, 유효 시간이 경과한 데이터는 자동으로 필터링해 노출되지 않도록 구현
    
<p align="left">
  <img src="https://github.com/user-attachments/assets/3740c587-d807-4d86-81e0-c935877a883d" width="20%" alt="Story">
  <img src="https://github.com/user-attachments/assets/079fd7be-f06e-4436-b941-445566ce8b77" width="20%" alt="DM">
</p>

#### 4️⃣ 실시간 DM 시스템 (Direct Message) 💬
유저와 캐릭터 간 1:1 채팅을 위한 메시지 송수신 시스템을 구축
- **주요 구현 :** 
  - **실시간 통신 :** 서버- AI간 데이터를 실시간 이벤트 기반으로 중계하여 대화 내용이 즉시 화면에 반영되는 대화형 인터페이스 구축
  - **채팅 UI 최적화 :** 가변형 말풍선 리스트 구현
  - **상태 표시 :** 지연 메세지, 메시지 읽음 처리(Read Receipt) 및 실시간 글쓰기 상태 표시 로직 연동

<p align="left">
  <img src="https://github.com/user-attachments/assets/d374e585-153a-4022-8083-3832dace2e4a" width="20%" alt="DM List">
  <img src="https://github.com/user-attachments/assets/079fd7be-f06e-4436-b941-445566ce8b77" width="20%" alt="DM">
</p>

## 💻 Code Snippets

### [Feed System]

<details> <summary><b>1️⃣ 피드 레이아웃 재조정</b></summary>
  
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

### [Story System]

<details> <summary><b>1️⃣ 스토리 및 상단 프로그레스 바 셋팅</b></summary>
  
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
  
<details> <summary><b>2️⃣ 다음 데이터 셋팅 </b></summary>
  
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
  
<details> <summary><b>3️⃣ 시간 만료 체크 </b></summary>
  
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

---
 
> **"복잡한 로직을 단순하고 견고하게 설계하는 것을 즐깁니다. 유저에게 즐거운 경험을 주기 위해 끊임없이 고민하는 클라이언트 개발자가 되겠습니다."**
