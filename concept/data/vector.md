
# Vector Embedding

정의 : 어떠한 개체(Text, Image, Audio, etc)의 묶음에 대해 개체간의 관계를 반영해 숫자의 목록(Vector)으로 변환하는 일

목적 : 상호 비교/연산이 가능하도록 개체를 숫자 혹은 Vector 정보로 변환 

# Embedding Model

- Text : Word2Vec, GLoVE, BERT, USE
- Image : CNNs (Convolutional Neural Networks), VGG, Inception
- Audio : Spectrogram을 사용한 (진동수별) 가시화를 통한 변형

# Similarity

개체를 비교하기 위해서는 개체를 비교가능하도록 숫자로 변환하는 과정과, 숫자간의 유사도를 검사하는 과정이 필요하며, 이를 위해서 Vector라는 단위를 사용한다.

Vector간의 유사도계산 과정을 '유사도 검색' 또는 'Vector 검색' 이라 부르며, Vector사이의 유사도는 다차원 공간에서 Vector간의 거리에 반비례한다.

Vector간의 거리계산을 위해 ML에서 사용되는 주요 측정방법(Metric)으로는 Euclidean(L2), Manhattan, Cosine, Chebyshev가 있다.

<img src=https://d33wubrfki0l68.cloudfront.net/05ba039181d2fb699d30257790c4b731e14de9ef/6db99/images/what-is-similarity-search-distance-metrics.jpeg width=650 height=600/>

# Searching

벡터정보가 데이터베이스에 저장되어 있고, 어떤 질의 벡터(Query Vector)가 입력되었을때 가장 유사한 벡터를 찾아주는 방법

검색은 입력된 Query의 metadata를 기반으로 `Vector index`에 대한 `Filtering`을 수행해 실제 유사도를 계산할 Vector를 간추리고, 이후 유사도를 계산해서 상위 N개를 반환하는 형태로 동작

예) K Nearest Neighbors (k-NN)
> k-NN은 질의 벡터가 주어졌을때, 한 공간에서 가장 근접한 벡터를 찾는 알고리즘이다.
> k-NN은 데이터베이스내 모든 벡터에 대해서 거리를 계산하는 과정이 필요하다는 단점을 가지기에, 대체방안으로 ANN(Approximately Nearest Neighbors)이 사용될 수도 있다.


## Filtering

검색으로 찾은 개체들 중 일치도가 높더라도 실제 원하는 결과가 아닐 수 있다. 따라서 필터링은 질의에대한 의미를 파악하고 의미에 적합한 분류를 정의해 의도에 맞는 분류의 개체만 간추려 볼 수 있도록 한다.

벡터 데이터베이스에서는 이러한 분류를 metadata 조건으로 기술하고, 이를 기반으로 검색의 범주를 제한하는 방법을 사용한다.

## Index

정적 혹은 기계학습으로부터 원본 데이터의 정보를 유용하고 의미있게 인코딩한 벡터 만들수 있으며,이러한 의미있는 벡터는 보다 효율적인 유사도 검색을 위해 인덱스라는 데이터로 정의해 사용한다.

Index로는 검색 속도, 품질, 메모리 등을 고려할 수 있으며, Faiss(Facebook AI Similarity Search)에서처럼 여러 Index를 다층구조로 정의해 사용할 수도 있다.

### 기법

1. Flat : 벡터에 변형을 가핮 않고 그대로 사용하며, 전체 벡터를 대상으로 검색하여 정확도는 높지만 속도가 느리다.

```Python
d = 128  # dimensionality of Sift1M data
k = 10  # number of nearest neighbors to return

index = faiss.IndexFlatIP(d)    # Inner Product distance 사용 (Euclidean/L2 거리는 IndexFlatL2 method 사용)
index.add(data)
D, I = index.search(xq, k)
```

2. LSH (Locality Sensitive Hashing) : 해싱(Hashing)이 가능한 충돌할 수 있도록 하는 해시 함수를 통해 각 벡터를 bucket 단위로 그룹화해두고 검색 과정에서 key값만으로 바로 찾을 수 있도록 하는 방법이다. 단, 고차원이나 데이터가 많은 경우에는 메모리 및 검색 비용이 크기때문에 적합하지 않다.

``` Python
nbits = d*4  # resolution of bucketed vectors

# initialize index and add vectors
index = faiss.IndexLSH(d, nbits)
index.add(wb)

# and search
D, I = index.search(xq, k)
```

3. HNSW (Hierarchical Navigable Small World) : NSW 그래프를 여러 계층으로 분할하는 방식이며, 상위 계층으로 갈수록 노드(Vertex)간의 중간 연결이 제거된 구조이다.

<img src=https://d33wubrfki0l68.cloudfront.net/1fcaebe70c031d408ae082da355bfe0c6ecc04ac/ba768/images/similarity-search-indexes16.jpg width=500 height=350/>

``` Python
# set HNSW index parameters
M = 64  # number of connections each vertex will have
ef_search = 32  # depth of layers explored during search
ef_construction = 64  # depth of layers explored during index construction

# initialize index (d == 128)
index = faiss.IndexHNSWFlat(d, M)
# set efConstruction and efSearch parameters
index.hnsw.efConstruction = ef_construction
index.hnsw.efSearch = ef_search
# add data to index
index.add(wb)

# search as usual
D, I = index.search(wb, k)
```

4. IVF (Inverted File Index) : 클러스터링을 통한 검색 범위를 축소하고, 클러스터링을 위해 Voronoi Diagram(Dirichlet tessellation)의 개념을 사용한다.

<img src=https://d33wubrfki0l68.cloudfront.net/c384231ba57345064a94e95b5a53c4ea3b79a3e4/d2aa9/images/similarity-search-indexes22.jpg width=500 height=350/>

<img src=https://d33wubrfki0l68.cloudfront.net/ef65dd6a83e8fa8ddb25e0000b7e2e53678e75e1/303f9/images/similarity-search-indexes26.png width=500 height=350/>

``` Python
nlist = 128  # number of cells/clusters to partition data into

quantizer = faiss.IndexFlatIP(d)  # how the vectors will be stored/compared
index = faiss.IndexIVFFlat(quantizer, d, nlist)   # nlist=생성할 cell(probe)의 수
index.train(data)  # we must train the index to cluster into cells
index.add(data)

index.nprobe = 8  # set how many of nearest cells to search
D, I = index.search(xq, k)
```


# 관련자료
- [Vector Embedding](https://www.pinecone.io/learn/vector-embeddings/)
- [Similarity Search](https://www.pinecone.io/learn/what-is-similarity-search/)
- [Vector Indexes](https://www.pinecone.io/learn/vector-indexes/)
