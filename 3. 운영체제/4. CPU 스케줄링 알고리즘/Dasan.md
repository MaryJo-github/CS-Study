# CPU 스케줄링 알고리즘

CPU 스케줄러는 CPU 스케줄링 알고리즘에 따라 프로세스에서 해야 하는 일을 **스레드 단위**로 CPU에 할당합니다. 프로그램이 실행될 때는 CPU 스케줄링 알고리즘이 어떤 프로그램에 CPU 소유권을 줄 것인지 결정합니다. 

### 알고리즘의 목표
- CPU 이용률은 높게
- 주어진 시간에 많은 일을 하게
- 준비 큐에 있는 프로세스는 적게
- 응답 시간은 짧게 설정하는 것

### CPU 스케줄링 알고리즘
- 비선점형
    - FCFS(First Come, First Served)
    - SJF(Shortest Job First)
    - 우선순위
- 선점형
    - 라운드 로빈(Round Robin)
    - SRF(Shortest Remaining Time First)
    - 다단계 큐

<br>

## 📍 비선점형 방식(non-preemptive)
- 프로세스가 스스로 CPU 소유권을 포기하는 방식이며, **강제로 프로세스를 중지하지 않음**
- 따라서 컨텍스트 스위칭으로 인한 부하가 적음

### FCFS(First Come, First Served)
- 가장 먼저 온 것을 가장 먼저 처리하는 알고리즘
- 단점: 길게 수행되는 프로세스 때문에 `convoy effect` (준비 큐에서 오래 기다리는 현상)이 발생

### SJF(Shortest Job First)
- **실행 시간이 가장 짧은** 프로세스를 가장 먼저 실행하는 알고리즘
- 평균 대기 시간이 가장 짧다. 하지만 실제로는 실행 시간을 알 수 없기 때문에 과거의 실행했던 시간을 토대로 추측해서 사용함
- 단점: `starvation` (긴 시간을 가진 프로세스가 실행되지 않는 현상)이 발생

### 우선순위
- 기존 SJF 스케줄링을 보완한 알고리즘
- **오래된 작업일수록 우선순위를 높이는 방법(aging)**을 사용하는 알고리즘

## 📍 선점형 방식(preemptive)
- 현대 운영체제가 쓰는 방식
- 지금 사용하고 있는 프로세스를 알고리즘에 의해 중단시켜 버리고 **강제로 다른 프로세스에 CPU 소유권을 할당하는 방식**

### 라운드 로빈(RR, Round Robin)
- 현대 컴퓨터가 쓰는 스케줄링인 우선순위 스케줄링의 일종
- **각 프로세스는 동일한 할당 시간을 주고** 그 시간 안에 끝나지 않으면 다시 준비 큐의 뒤로 가는 알고리즘
    - 할당 시간이 너무 크면 →  FCFS가 됨
    - 할당 시간이 짧으면 → 컨텍스트 스위칭이 잦아져서 오버헤드, 즉 비용이 커짐
- 일반적으로 전체 작업 시간은 길어지지만 평균 응답 시간은 짧아짐
- 이 알고리즘은 로드밸런서에서 트래픽 분산 알고리즘으로도 사용됨

### SRF(Shortest Remaining Time First)
- **중간에 더 짧은 작업이 들어오면** 수행하던 프로세스를 중지하고 해당 프로세스를 수행하는 알고리즘
    - SJF: 중간에 실행 시간이 더 짧은 작업이 들어와도 기존 짧은 작업을 모두 수행하고 그 다음 짧은 작업을 이어나감

### 다단계 큐
- **우선순위에 따른 준비 큐를 여러 개 사용**하고, 큐마다 라운드 로빈이나 FCFS 등 다른 스케줄링 알고리즘을 적용한 것
- 장점: 큐 간의 프로세스 이동이 안되므로 스케줄링 부담이 적음
- 단점: 유연성이 떨어짐
