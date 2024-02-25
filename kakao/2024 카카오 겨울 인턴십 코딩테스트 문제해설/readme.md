### 240225
## [2024 카카오 겨울 인턴십 코딩테스트 문제해설](https://tech.kakao.com/2023/12/27/2024-coding-test-winter-internship/)
### 어느 정도를 해야 하나 그냥 재미로 하나만 풀어봤다.
### <br/>

## 1번 문제 : 가장 많이 받은 선물
### 다른 사람은 어떻게 풀었나 한 번 봤는데 대체로 50줄 내였다.
### 나는 목적 마다 함수화를 하였고, 중간중간마다 공백만 거의 30줄이다.
### 그리고 문제에서 제한 사항으로 이런 이런 케이스는 안 됩니다 라고 써놨는데, 그러한 data_validation 작업을 다른 사람들은 안 해놨다는 게 신기했다. 오로지 정답을 위한 문제 풀이에 집중했다.
### 5시간 안에 5문제 풀어야 하는데 1번 문제 푸는데 1시간 걸렸다... 나는 시간 제한으로 아슬아슬할 듯...

### <br/>
### 코드
```
def recieve_next_m(dicScore, sFriend_A, sFriend_B) : 
    if dicScore[sFriend_A]['give'][sFriend_B] > dicScore[sFriend_B]['give'][sFriend_A] : 
        dicScore[sFriend_A]['recieve_next_m'] += 1
    elif dicScore[sFriend_A]['give'][sFriend_B] == dicScore[sFriend_B]['give'][sFriend_A] : 
        if dicScore[sFriend_A]['present_score'] > dicScore[sFriend_B]['present_score'] : 
            dicScore[sFriend_A]['recieve_next_m'] += 1
        elif dicScore[sFriend_A]['present_score'] < dicScore[sFriend_B]['present_score'] : 
            dicScore[sFriend_B]['recieve_next_m'] += 1
    else : 
        dicScore[sFriend_B]['recieve_next_m'] += 1

    return dicScore

    
def friends_validation(ls) : 
    ls = list(map(lambda x : x.lower(), ls))
    
    blValidation = True
    if len(ls) < 2 or len(ls) > 50 : 
        blValidation = False
    
    for i in range(0, len(ls)) : 
        if len(ls[i]) > 10 : 
            blValidation = False
    
    if len(ls) != len(set(ls)) : 
        blValidation = False
    
    return blValidation


def gift_validation(lsGift, lsFriend) : 
    dicFriend = dict.fromkeys(lsFriend, 0)
    blValidaton = True
    if len(lsGift) < 1 or len(lsGift) > 10000 : 
        blValidaton = False
    
    for i in range(0, len(lsGift)) : 
        lsGift_friend = lsGift[i].split(" ")
        if len(lsGift_friend) != len(set(lsGift_friend)) or len(lsGift_friend) != 2 : 
            blValidaton = False
        for j in range(0, len(lsGift_friend)) : 
            if lsGift_friend[j] not in dicFriend.keys() : 
                blValidaton = False
    
    return blValidaton


def make_data(lsG, dicScore) : 
    for i in range(0, len(lsG)) : 
        lsG_sub = lsG[i].split(" ")
        dicScore[lsG_sub[0]]['give_score'] += 1
        dicScore[lsG_sub[1]]['recieve_score'] += 1
        dicScore[lsG_sub[0]]['give'][lsG_sub[1]] += 1
    
    for i in dicScore.keys() : 
        dicScore[i]['present_score'] = dicScore[i]['give_score'] - dicScore[i]['recieve_score']
        
    return dicScore
    
    
def solution(friends, gifts) :
    if friends_validation(friends) == False or gift_validation(gifts, friends) == False : 
        print("데이터 형식이 맞지 않습니다.")
        return -1
    
    dicScore = {i : {'give' : dict.fromkeys(friends, 0).copy(), 'give_score' : 0, 'recieve_score' : 0, 'present_score' : 0, 'recieve_next_m' : 0} for i in friends} 
    dicScore = make_data(gifts, dicScore)
    
    n = 1
    for i in range(0, len(friends) - 1) : 
        for j in range(n, len(friends)) :  
            dicScore = recieve_next_m(dicScore, friends[i], friends[j])
        n += 1
    answer = max(list(map(lambda x : dicScore[x]['recieve_next_m'], dicScore)))
    
    return answer


def main() : 
    # test 1. answer = 2
    friends = ["muzi", "ryan", "frodo", "neo"]
    gifts = ["muzi frodo", "muzi frodo", "ryan muzi", "ryan muzi", "ryan muzi", "frodo muzi", "frodo ryan", "neo muzi"]

    # test 2. answer = 4
    friends = ["joy", "brad", "alessandro", "conan", "david"]
    gifts = ["alessandro brad", "alessandro joy", "alessandro conan", "david alessandro", "alessandro david"]

    # test 3. answer = 0
    friends = ["a", "b", "c"]
    gifts = ["a b", "b a", "c a", "a c", "a c", "c a"]
    answer = solution(friends, gifts)

    print(answer)


main()
```

https://github.com/Shin-jongwhan/tech_blog/assets/62974484/87d689e3-f09d-45f5-bb8b-eb16e9a036f0

