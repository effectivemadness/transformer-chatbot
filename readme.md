# simple chatbot model using transformer

- Data
    - https://github.com/songys/Chatbot_data

- Usage
    - python preprocess_run.py
    - python transformer.py

- Preprocess
    - EDA - Exploratory Data Analysis
        - 데이터의 길이, 평균, 분포, 중간값 등 찾아보기
        - outlier 처리 방법 찾기.
        - 문장 구성요소 중 마침표, 물음표, 느낌표 등 "모두가 가지고 있는 특징은 제거하는 방향"
    - 자연어 처리 데이터 전처리
        - Data Clean
            - 필요없는 문자 제거 - 긁어올 때 찌꺼기(HTML tag, etc) 제거 - Beautifulsoup
            - 필요없는 문자 제거 - 영문자는 전부 소문자 처리 or 한글만 남기기 - regex
            - 불용어 처리 - ex. is, am, 감탄사, 조사 etc. - NLTK 불용어 사전 or 자체 제작 불용어 사전

- Transformer
    - Attention 구조만으로 전체 모델 구성.
    - Self attention → 문장에서 각 단어끼리 얼마나 관계가 있는지 계산해 반영.
    - 모듈
        - Multi-head attention
            - 스케일 내적 어텐션
                - query, key, value → query 받아 key로 나머지 단어들 과의 어텐션 스코어(value) 가중합.
                - 중간에 scailing →벡터의 차원이 커지면 학습이 잘 안될수도. 크기 스케일링.
            - 순방향 마스크 어텐션
                - 디코더 부분의 경우, 자신 외의 다른 모든 단어들로 예측 → 예측하지 않은 뒤의 단어들은 참고 못하게 마스킹
            - 멀티 헤드 어텐션
                - query, key, value에 대한 특징값 헤드 수만큼 나눠 linear layer 거쳐 scaled dot-product 구해 합침 → attention map을 여러개 만들어 다양한 특징에 대한 어텐션을 볼 수 있게 함.
        - Position-wise Feedforward Network
            - Self Attention 레이어를 거친 후.
            - 한 문장에 있는 단어 토큰 각각에 대해 연산.
        - Residual connection
            - resnet에서 쓰는 느낌의 레이어.
            - FeedForwardNetwork 통과하기 전, 후 합. → 합하고 나서 layer normalization 진행.
        - Positional Encoding
            - 하나하나 순차적으로 넣는게 아니고 한 번에 넣는 Transformer 모델 구조.
            - 입력 시퀀스 정보에 대한 순서 정보 주입 필요.

- "텐서플로 2와 머신러닝으로 시작하는 자연어 처리" 책의 [코드](https://github.com/NLP-kr/tensorflow-ml-nlp-tf2)를 기반으로 기록용으로 작성하였습니다.