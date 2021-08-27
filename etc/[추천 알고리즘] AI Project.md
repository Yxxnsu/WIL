# [추천 알고리즘] : AI Project

# Recommend with Collaborative Filtering

## [1] using collaborative filtering : Item - based ( 아이템 기반 )

collaborative filtering 이란 추천 시스템에서 사용하는 기술입니다.                                     

#Link

[협업 필터링 추천 시스템 (Collaborative Filtering Recommendation System)](https://scvgoe.github.io/2017-02-01-%ED%98%91%EC%97%85-%ED%95%84%ED%84%B0%EB%A7%81-%EC%B6%94%EC%B2%9C-%EC%8B%9C%EC%8A%A4%ED%85%9C-(Collaborative-Filtering-Recommendation-System)/)

#Code

```python
#Using Surprise.
#Surprise 는 추천 시스템을위한 사용하기 쉬운 Python scikit
#Reference : https://dschloe.github.io/python/recommendation/recommendation_02/

#Surprise에서 읽기
#Reader() 클래스는 user : item : rating 구조를 필요로 한다.
reader = Reader()

# get just top 100K rows for faster run time
# Pandas의 Dataframe에서 데이터를 로딩한다. 
# 마찯가지로 반드시 3개의 Column인 uid / ItemId / rating 순이다. 그리고 Reader로 파일 포멧
data = Dataset.load_from_df(df[['Cust_Id', 'Movie_Id', 'Rating']][:], reader)
#data.split(n_folds=3)

#여기까지 데이터 변환
-----------------------------------

#SVD 모형 학습 = 단일 값 분해를 수행하는 모델 훈련
#3중 교차 검증을 사용
svd = SVD()
cross_validate(svd, data, measures=['RMSE', 'MAE'])
```

## [2]What user 783514 liked in the post.

#Code 

Des: 785314 유저가 Rating == 5를 준 데이터들을 가지고 온다.

```python
#Cust_Id 가 785314 && Rating이 5인 열을 df_785314에 저장

df_785314 = df[(df['Cust_Id'] == 785314) & (df['Rating'] == 5)]

#Movie_Id 추가하고 Title을 DateFrame에 추가
df_785314 = df_785314.set_index('Movie_Id')
df_785314 = df_785314.join(df_title)['Name']
print(df_785314)
```

## [3] Let`s predict which moives user 785314 would love to watch

#Code

Des: 785314 유저가 좋아하는 영화가 무엇인지 예측

```python
user_785314 = df_title.copy()
user_785314 = user_785314.reset_index()
user_785314 = user_785314[~user_785314['Movie_Id'].isin(drop_movie_list)]

# getting full dataset
data = Dataset.load_from_df(df[['Cust_Id', 'Movie_Id', 'Rating']], reader)

#build_full_trainset method를 사용하여 교차 검증을 위한 데이터 세트를 fit method를 사용하여
#전체 데이터 세트에서 모델을 훈련한다.
trainset = data.build_full_trainset()
svd.fit(trainset)

#훈련된 SVD 모델을 사용하여 유저ID와 x 가 주어지면 유저가 영화에 할당 할 평점을 예측할 수 있다.
# "predict" method로 이를 수행했다.
user_785314['Estimate_Score'] = user_785314['Movie_Id'].apply(lambda x: svd.predict(785314, x).est)

#열( Column )으로 나열
user_785314 = user_785314.drop('Movie_Id', axis = 1)

#평점이 높은 순서로 정렬 ( Sort )
user_785314 = user_785314.sort_values('Estimate_Score', ascending=False)
print(user_785314.head(10))
```

## [4] Recommend with Pearsons` R corrlations

#Code

```python
/*Recommand method*/
#The way it works is we use Pearsons' R correlation to measure 
#the linear correlation between review scores of all pairs of movies
#I can`t understand this code boom ~

def recommend(movie_title, min_count):
    print("For movie ({})".format(movie_title))
    print("- Top 10 movies recommended based on Pearsons'R correlation - ")
    i = int(df_title.index[df_title['Name'] == movie_title][0])
    target = df_p[i]

		#모든 변수간 상관계수 구하기 (Pandas method)
    similar_to_target = df_p.corrwith(target)
		
		#구한 상관계수를 PearsonR 열에 넣는다.
    corr_target = pd.DataFrame(similar_to_target, columns = ['PearsonR'])
    
		#결측값 (Ex. NaN) 제거 
    corr_target.dropna(inplace = True)

		#높은 값부터 순서대로 Sort
		#상관계수가 높을수록 선택한 영화와 비슷한 영화
    corr_target = corr_target.sort_values('PearsonR', ascending = False)
    
		corr_target.index = corr_target.index.map(int)
    corr_target = corr_target.join(df_title).join(df_movie_summary)[['PearsonR', 'Name', 'count', 'mean']]
    print(corr_target[corr_target['count']>min_count][:10].to_string(index=False))

```

#Result

recommend("What the #$*! Do We Know!?", 0)

---

```python
/*Result*/

For movie (What the #$*! Do We Know!?)
- Top 10 movies recommended based on Pearsons'R correlation - 
 PearsonR                                      Name  count      mean
 1.000000                What the #$*! Do We Know!?  14910  3.189805
 0.505500                                 Inu-Yasha   1883  4.554434
 0.452807  Captain Pantoja and the Special Services   1801  3.417546
 0.442354                 Without a Trace: Season 1   2124  3.980226
 0.384179                      Yu-Gi-Oh!: The Movie   3173  3.331547
 0.383959                                  Scorched   2430  2.894239
 0.381173   All Creatures Great and Small: Series 1   2327  3.938118
 0.381112           As Time Goes By: Series 1 and 2   2249  4.164073
 0.373018                          Cowboys & Angels   2368  3.589527
 0.371981                            Biggie & Tupac   1866  3.019293
```

## Reference

- [https://towardsdatascience.com/how-you-can-build-simple-recommender-systems-with-surprise-b0d32a8e4802](https://towardsdatascience.com/how-you-can-build-simple-recommender-systems-with-surprise-b0d32a8e4802)
- https://dschloe.github.io/python/recommendation/recommendation_02/
- [https://scvgoe.github.io/2017-02-01-협업-필터링-추천-시스템-(Collaborative-Filtering-Recommendation-System)](https://scvgoe.github.io/2017-02-01-%ED%98%91%EC%97%85-%ED%95%84%ED%84%B0%EB%A7%81-%EC%B6%94%EC%B2%9C-%EC%8B%9C%EC%8A%A4%ED%85%9C-(Collaborative-Filtering-Recommendation-System)/)/
- [https://yeo0.github.io/data/2019/02/21/Recommendation-System_Day6/](https://yeo0.github.io/data/2019/02/21/Recommendation-System_Day6/)
- 상관계수: [https://rfriend.tistory.com/405](https://rfriend.tistory.com/405)

[[AI] Project Analysis](https://www.notion.so/AI-Project-Analysis-6c1c7f6bf2b14185a13960806b5563a6)