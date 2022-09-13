---
title:  "한 대상에 대해 여러가지 데이터가 주어질 때, 구조체와 벡터 활용해 여러 대상 정렬하기 "

categories:
  - PracticingCpp
tags:
  - PracticingCpp
  
last_modified_at: 2022-01-14T08:06:00-05:00
---

문제를 풀다 보면 여러 대상이 주어지고  대상들이 가진 특성들이 입력이나 데이터로 주어질때, 목적 대상을 구하기 위해 대상들이 가진 특성들에 대한 조건에 따라 대상들을 정렬해야 하는 상황이 있습니다.
(데이터들이 하나의 string형 데이터로 주어진다면 데이터들을 분리하여 저장해야 합니다.)

[하나의 스트링 입력으로부터 데이터 추출하기](/) 

이렇게, 한 대상에 대해 여러 정보가 주어지는 경우에는 구조체를 이용해 한 대상에 대해 여러가지 타입의 정보(특성)들을 저장할 수 있고, vector와 sort함수를 활용해 특성에 따른 정렬기준을 자신이 커스터마이징하여 대상들을 정렬할 수 있습니다. 


백준에서 가져온 문제를 예시로 보겠습니다.

[백준 11946번](https://www.acmicpc.net/problem/11946)

## 문제 설명
프로그래밍 대회에서는 대략 10개 내외로 문제가 주어지며 5시간동안 주어진 문제들을 최대한 많이 풀어서 통과해야 한다. 만약 틀린 답안을 제출했을 경우 다음과 같은 피드백을 준다.

  run-time error

  time-limit exceeded
  
  wrong answer

## 다음은 등수 산정 방식을 설명한 것이다.

    1. 각 팀의 등수는 문제를 많이 푼 순으로 산정된다.
    
    2. 만약 푼 문제 수가 같을 경우 ‘총 시간’이 작은 순으로 산정된다.
    ‘총 시간’은 각 문제를 풀어서 통과했을 때의 시간 값을 더한 것이다.
    
    3. 각 문제를 풀어서 통과하면 대회 시작으로부터 통과할 때까지 걸린 분 수와 그 문제를 통과하기 이전의 틀린 답을 제출한 횟수에 20을 곱한 패널티를 더한다.  

    4.통과하지 못한 문제에 대한 패널티는 계산하지 않는다.

    5.만약 푼 문제 수와 총 시간까지 같을 경우 같은 등수로 처리하도록 하자
    
    6.이미 통과한 문제는 다시 제출해서 틀리거나 맞더라도 결과에 반영되지 않는다(즉 처음 통과할 때 ‘총 시간’을 적용하도록 한다).

## 입력
입력의 첫 줄에는 대회에 참가한 팀의 수를 나타내는 n과(1<=n<=100) 문제 수 m(1<=m<=15), 그리고 채점 로그의 개수 q가 주어진다(0<=q<=10000).

이후에 오는 q개의 줄에는 각 채점 정보가 주어진다. 채점 정보는 경과 시간(300(5시간)이하의 음이 아닌 정수), 팀 번호(1부터 n까지의 자연수), 문제 번호(1부터 m까지의 자연수), 그리고 채점 결과로 이루어져 있다. 

채점 결과는 “AC”, “RE”, “TLE”, “WA”중 하나이며 각각 “Accepted”, “Run-time Error”, “Time-limit Exceeded”, “Wrong Answer”를 의미한다(“Accepted”가 통과를 의미하며 나머지는 오답으로 간주된다). 채점 정보는 채점된 순서대로 입력된다. 즉 경과 시간 순으로 입력될 것이다.

## 출력
대회가 끝난 후 팀 별 성적을 순위에 따라 출력한다. 만약 순위가 같으면 팀 번호가 빠른 것을 먼저 출력한다. 각 팀마다 팀 번호, 푼 문제 수, 총 시간을 출력한다.

## 풀이
이 문제에서는 팀이 대상으로서 주어지며 순위를 결정하는 요인들을 정리해 봅시다, 

__팀이 푼 문제 수__ , __팀의 총 시간__ 이 순위를 결정하며 순위가 같을 경우 __팀번호__ 가 출력 순위에 영향을 미치고, 산정방식 3,4,6번에 의해 각 팀에 대해 __각 문제의 해결 여부__ 와 __각 문제에 대한 누적 패널티__ 정보도 필요하므로 이 대상 특성들을 하나의 구조체로 묶습니다. 

정의한 구조체를 원소로 가지는 크기가 N(팀의 개수)인 벡터를 만들고, 출력에 영향을 미치는 구조체의 멤버들에 대해서 조건에 주어진 대로 정렬할 수 있도록 비교 함수를 만듦니다.  

__문제의 해결여부__ 와 __누적 패널티__ 는 채점 결과에 따라 총시간에 영향을 주므로 if-else문으로 분기 처리하여 총시간에 대한 결과를 갱신해 나갑니다.  

```cpp
//string정보를 받으므로 #include<iostream>을 포함시킵니다.
#include <iostream>
#include<vector>
#include<string>
#include<algorithm>
using namespace std;

int N,M,Q ;//the number of participants, problem, checklog 

//구조체 정의 
struct info{
	int teamNum;
	int solvedNum;
	int totalTime;
	vector<int>problem;//각 문제당 누적 패널티
	vector<int>solved;//푼 문제를 풀었는지 확인

  //생성자 정의 =>구조체 원소를 할당하거나 추가했을 때 멤버의 디폴트 값을 설정해 줍니다. 
  	info(int X) : problem(X), solved(X) { //X는 사용자가 넘겨주는 값
		teamNum = solvedNum = totalTime = 0;
	}
  //problem과 solved 멤버는 벡터이므로 첫 인자로 vector의 크기 값 X를 넘겨 받습는다, 
  //벡터에서는 두번째 인자로 벡터 원소의 공통 할당 값을 입력받을 수 있습니다.
  //(2번째 인자를 주지 않으면 워소의 값은 0으로 초기화)
  //나머지는 0으로 setting합니다.     
};


//멤버에 따른 정렬기준 정하기
bool cmp(const info & prev,const info & next){
	if(prev.solvedNum==next.solvedNum){ //푼 문제가 같을 경우
			if( prev.totalTime==next.totalTime)//총 시간이 같을 경우
				return prev.teamNum<next.teamNum;//순위가 같으면 팀번호로 출력순위 결정
		
			return prev.totalTime<next.totalTime;
	}
	
	return prev.solvedNum>next.solvedNum;
}


int main() {
	cin>>N >>M >>Q;
	vector<info> Info(N,info(M));
	//구조체 벡터 초기화
	for(int i=0;i<N;i++){
		Info[i].teamNum=i+1;//팀 번호 초기화

		//생성자를 이용하지 않았다면 아래 값을 모두 초기화해줘야 합니다.
    // Info[i].solvedNum=0;
		// Info[i].problem.resize(M+1);
		// Info[i].solved.resize(M+1);
		// Info[i].totalTime=0;
	}
		
	
	for (int i=0;i<Q;i++){
		int  cur_time,team,prob;
		string result;

		cin >> cur_time>>team >>prob >> result;
		
		if(result=="AC"){//문제를 맞힌 경우
			if(!Info[team-1].solved[prob]){
			Info[team-1].solved[prob]=true;
			Info[team-1].solvedNum++; //문제를 맞힌 개수 증가
			Info[team-1].totalTime+=cur_time+Info[team-1].problem[prob];//현 시간+ 쌓인 패널티
			}
		}
		
		else  //맞히지 못한 경우
			if(!Info[team-1].solved[prob])
			    Info[team-1].problem[prob]+=20;//틀린 문제에 대해서 패널티 부여
	}
	
	sort(Info.begin(),Info.end(),cmp);// 문제의 기준에 맞게 커스텀한 비교함수대로 정렬
	
	for(auto &i:Info)
		cout<<i.teamNum <<" "<<i.solvedNum <<" "<<i.totalTime<<endl;
}
```