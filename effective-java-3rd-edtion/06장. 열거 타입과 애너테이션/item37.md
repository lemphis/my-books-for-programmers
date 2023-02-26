# 37. ordinal 인덱싱 대신 EnumMap을 사용하라

배열의 인덱스를 얻기 위해 ordinal을 쓰는 것은 일반적으로 좋지 않다.  
대신 `EnumMap`을 사용하라.  
다차원 관계는 EnumMap\<..., EnumMap\<...>> 으로 표현하라.