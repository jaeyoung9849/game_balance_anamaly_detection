# 밸런스 이상 캐릭터 탐지

## :sunny: 소개
- A게임은 **10명의 플레이어가 두 팀으로 나누고, 각자의 캐릭터**를 골라 전투를 벌이는 **팀 배틀 AOS 게임** 입니다.
- **캐릭터의 스테이터스가 과도하게 좋거나 나쁘다면** 캐릭터 간 **밸런스가 맞지않아 게임의 퀄리티가 저하**됩니다.
- 따라서 **스테이터스 이상 캐릭터를 탐지**하고 **밸런스 조정**을 통해 게임의 **완성도를 높여야** 합니다.
- 10개의 Column과 44개(캐릭터 갯수)의 데이터를 분석했습니다. 

![image](https://user-images.githubusercontent.com/56102116/227695708-3558aa9d-2c4d-43a9-9ed9-02ce6639f6e6.png)

<br/>

----------------------------

## :sunny: 결론

- **같은 Role인데도 불구**하고 능력치에 **많은 차이**를 보인 **All-Rounder, Attacker, Defender캐릭터의 밸런스 조정이 필요**합니다.
- **Feature extraction**으로 **공격력, 지구력, 이동력, 득점력, 지원력을 다 더한 all_stat**이라는 **새로운 feature를 생성**하여 비교했습니다.

![image](https://user-images.githubusercontent.com/56102116/227690610-35048554-be7a-401a-ac83-7a1121cb4a6e.png)

<br/>

### 1) All-Rounder 밸런스 조정

![image](https://user-images.githubusercontent.com/56102116/227696742-64c655e3-4b85-4456-a335-c32054bfa099.png)

- **Scizor가 Endurance를 제외한 모든 분야에서 Azumarill과 Tsareena보다 더 강한 수치**를 보입니다.
- 따라서 **Scizor의 능력치를 줄이거나 Azumarill와 Tsareena의 능력치를 강화**하는 밸런스 조정이 필요합니다.

<br/>

### 2) Attacker 밸런스 조정

![image](https://user-images.githubusercontent.com/56102116/227696756-20dfd9cc-e95f-4c34-a71e-9f79ca3fb93e.png)

- **Mew가 모든 영역에서 Glaceon보다 좋은** 능력치를 보입니다.
- 따라서 **Mew와 Glaceon의 차이를 줄이는** 밸런스 조정이 필요합니다.

<br/>

### 3) Defender 밸런스 조정

![image](https://user-images.githubusercontent.com/56102116/227696770-03cd00f7-1a3a-4709-be0d-0248e439647f.png)

- **Trevenant가 Offense를 제외한 모든 분야에서 Greedent보다 높은** 수치를 보입니다.
- 따라서 **Trenenant와 Greedent의 능력치 차이를 줄이거나 Greedent의 스킬 강화 등**의 밸런스 조정이 필요합니다.

<br/>
<br/>

---------------------------

## :sunny: EDA

### 1) 카테고리별 캐릭터 갯수 확인

![image](https://user-images.githubusercontent.com/56102116/227696351-95fff11e-b05d-424e-8fbe-cdf304f4ffc0.png)

- **All-Rounder(11), Attacker(14), Defender(7), Speedster(6), Supporter(6) 총 44개**의 캐릭터가 존재합니다.
- **근접(Melee)은 25개, 원거리(Ranged)는 19개**의 캐릭터가 존재합니다.
- **어려움(Expert)은 12개, 중간(Intermediate)는 18개, 초보(Novice)는 12개**의 캐릭터가 존재합니다.

<br/>

### 2) 캐릭터 역할별 세부 능력치 분석

![image](https://user-images.githubusercontent.com/56102116/227696598-b7902a7f-82d9-4b98-aa4e-19858d8453b0.png)

- All-Rounder라면 모든 능력치에서 튀지않고 적당한 능력치를 보여야 하지만, **대체적으로 공격력이 높고 지원력이 낮은것**으로 나타나 밸런스가 의심됩니다.
- 밸런스 이상 캐릭터를 탐지하기 위해 **역할별로 나누어 기준을 만들 필요**가 있습니다.

<br/>
<br/>

-----------------------------------

## :sunny: 모델링
- **Feature extraction**으로 **공격력, 지구력, 이동력, 득점력, 지원력을 다 더한 all_stat**이라는 **새로운 feature를 생성**하여 비교했습니다.
- **역할이 같은 캐릭터 내에서 all_stat의 차이가 존재하는지** 분석했습니다.

![image](https://user-images.githubusercontent.com/56102116/227690610-35048554-be7a-401a-ac83-7a1121cb4a6e.png)

- Speedster와 Supporter의 차이는 작은 데 반해, **All-Rounder와 Attacker와 Defender의 차이가 크게 발생**해 밸런스의 조정이 필요합니다.

<br/>

### 1) All-Rounder 비교

![image](https://user-images.githubusercontent.com/56102116/227698194-63ad71a9-e406-43fb-bb91-fafdc3d88f34.png)

- 같은 근거리 캐릭터인데도 불구하고 능력치의 **총합이 3.5 차이**가 발생하고 있습니다.

<br/>

### 2) Attacker 비교

![image](https://user-images.githubusercontent.com/56102116/227699957-d0eb6d47-bc32-40c2-a193-dd586f3e0f04.png)

- 같은 원거리 캐릭터인데도 불구하고 능력치의 **총합이 3.5 차이**가 발생하고 있습니다.

<br/>

### 3) Defender 비교

![image](https://user-images.githubusercontent.com/56102116/227700088-672c99da-9989-40ce-9078-a073037c8941.png)

- 같은 근거리 캐릭터인데도 불구하고 능력치의 **총합이 3.5 차이**가 발생하고 있습니다.

<br/>
<br/>

---------------------

## :sunny: 한계점

- **각 능력치를 합산한 값**을 기준으로 이상치를 판단했지만, **유관부서와 고객의 피드백을 종합적으로 판단**하여 더 객관적인 기준을 마련할 필요가 있습니다.
- 각 캐릭터의 능력치뿐만 아니라 **사용하는 스킬의 강도 등**을 알 수 있다면 더 완성도 높은 밸런스 검증이 가능할 것입니다.

<br/>
<br/>


