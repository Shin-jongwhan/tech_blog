### 240225
## [2024 카카오 겨울 인턴십 코딩테스트 문제해설](https://tech.kakao.com/2023/12/27/2024-coding-test-winter-internship/)
### 어느 정도를 해야 하나 그냥 재미로 하나만 풀어봤는데 그렇게 어렵지는 않다.
### <br/>

## 1번 문제 : 가장 많이 받은 선물
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
    #print(dicScore)
    #print(sFriend_A, dicScore[sFriend_A], sFriend_B, dicScore[sFriend_B])

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
        #print(lsG_sub)
        dicScore[lsG_sub[0]]['give_score'] += 1
        dicScore[lsG_sub[1]]['recieve_score'] += 1
        dicScore[lsG_sub[0]]['give'][lsG_sub[1]] += 1
        #print(dicScore)
    
    for i in dicScore.keys() : 
        dicScore[i]['present_score'] = dicScore[i]['give_score'] - dicScore[i]['recieve_score']
        
    return dicScore
    
    
def solution(friends, gifts) :
    if friends_validation(friends) == False or gift_validation(gifts, friends) == False : 
        print("데이터 형식이 맞지 않습니다.")
        return -1
    
    # dict.fromkeys(friends, {'give' : dict.fromkeys(friends, 0), 'give_score' : 0, 'recieve_score' : 0, 'present_score' : 0, 'recieve_next_m' : 0})
    dicScore = {} 
    for i in range(0, len(friends)) : 
        dicScore[friends[i]] = {'give' : dict.fromkeys(friends, 0).copy(), 'give_score' : 0, 'recieve_score' : 0, 'present_score' : 0, 'recieve_next_m' : 0}
    #print(dicScore)
    dicScore = make_data(gifts, dicScore)
    #print(dicScore)
    
    #print("")
    n = 1
    for i in range(0, len(friends) - 1) : 
        for j in range(n, len(friends)) :  
            if i == j : 
                continue
            dicScore = recieve_next_m(dicScore, friends[i], friends[j])
        n += 1
    #print(list(map(lambda x : dicScore[x]['recieve_next_m'], dicScore)))
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

