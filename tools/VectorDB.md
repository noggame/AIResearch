
# Vertor Database

Vector 검색은 사용자가 저장된 개체애 대한 


## Vector Embedding

정의 : 어떠한 개체(Text, Image, Audio, etc)의 묶음에 대해 개체간의 관계를 반영해 숫자의 목록(Vector)으로 변환하는 일

목적 : 상호 비교/연산이 가능하도록 개체를 숫자 혹은 Vector 정보로 변환 

### Embedding Model

- Text : Word2Vec, GLoVE, BERT, USE
- Image : CNNs (Convolutional Neural Networks), VGG, Inception
- Audio : Spectrogram을 사용한 (진동수별) 가시화를 통한 변형

## Similarity

개체를 비교하기 위해서는 개체를 비교가능하도록 숫자로 변환하는 과정과, 숫자간의 유사도를 검사하는 과정이 필요하며, 이를 위해서 Vector라는 단위를 사용한다.

Vector간의 유사도계산 과정을 '유사도 검색' 또는 'Vector 검색' 이라 부르며, Vector사이의 유사도는 다차원 공간에서 Vector간의 거리에 반비례한다.

Vector간의 거리계산을 위해 ML에서 사용되는 주요 측정방법(Metric)으로는 Euclidean, Manhattan, Cosine, Chebyshev가 있다.

<img src=https://d33wubrfki0l68.cloudfront.net/05ba039181d2fb699d30257790c4b731e14de9ef/6db99/images/what-is-similarity-search-distance-metrics.jpeg width=650 height=600/>

##  Search

벡터정보가 데이터베이스에 저장되어 있고, 어떤 질의 벡터(Query Vector)가 입력되었을때 가장 유사한 벡터를 찾아주는 방법

예) K Nearest Neighbors (k-NN)
> k-NN은 질의 벡터가 주어졌을때, 한 공간에서 가장 근접한 벡터를 찾는 알고리즘이다.
> k-NN은 데이터베이스내 모든 벡터에 대해서 거리를 계산하는 과정이 필요하다는 단점을 가지기에, 대체방안으로 ANN(Approximately Nearest Neighbors)이 사용될 수도 있다.


# 관련자료
- [벡터 검색의 종류와 활용도](https://www.itworld.co.kr/tags/18955/ai/209202)
- [Vector Database](https://www.pinecone.io/learn/vector-database/)
- [Vector Embedding](https://www.pinecone.io/learn/vector-embeddings/)
- [Similarity Search](https://www.pinecone.io/learn/what-is-similarity-search/)

