# 스케줄러
프로세스를 스케줄링하기 위한 큐에는 세가지 종류가 존재한다.
- Job Queue: 현재 시스템 내에 있는 모든 프로세스의 집합
- Ready Queue: 현재 메모리 내에 있고, CPU 시간을 할당받기를 기다리는 프로세스의 집합
- Device Queue: Device I/O작업을 대기하고 있는 프로세스의 집합
이러한 각각의 큐에 프로세스들을 넣고 빼주는 스케줄러에도 세가지 종류가 존재한다.

## 장기 스케줄러
> 사용할 수 있는 메모리들은 한정되어 있는데 많은 프로세스들이 한꺼번에 메모리에 올라올 경우, 디스크에 임시로 저장된다. 여기 저장되어 있는 프로세스중 어떤 프로세스를 Ready Queue로 보낼지 결정하는 역할을 한다.

- 메모리와 디스크 사이의 스케줄링을 담당한다.
- 프로세스에 메모리를 할당한다.
- 실행중인 프로세스의 수(degree of Multiprogramming)을 제어한다. <-- 중요!
- 프로세스의 상태는 `new -> ready`

## 단기 스케줄러
> 메모리에 올라와 있는 프로세스 중 어떤 프로세스에게 CPU를 할당할지를 결정한다.

- 메모리와 CPU사이의 스케줄링을 담당한다.
- Ready Queue에 있는 프로세스 중 어떤 프로세스를 running시킬지 결정한다.
- 프로세스에 CPU를 할당한다.(`scheduler dispatch`)
- 프로세스의 상태는 `ready -> running -> waiting -> ready`

## 중기 스케줄러
- 여유공간의 마련을 위해 프로세스를 통째로 메모리에서 디스크로 좇아낸다(swapping), 다른말로하면 프로세스를 메모리에서 해제(deallocated)시킨다.
- degree of Multiprogramming을 제어하기 위해서 사용한다.
- 현재 시스템에서 메모리에 너무 많은 프로그램이 올라오는 것을 제어하기 위해서 사용한다.
- 프로세스의 상태는 `ready -> suspended`

## 프로세스의 suspended 상태?
- 외부적인 이유로 프로세스의 수행이 정지된 상태. 메모리에서 완전히 내려간 상태를 말한다.
- 메모리에서 디스크로 전부 `swap out`된다.
- blocked된 상태는 Device I/O작업을 기다리는 상태이기 때문에 스스로 ready 상태로 돌아갈 수 있지만 이 상태는 외부적인 이유로 suspending되었기 때문에 스스로 돌아갈 수 없다.

[처음으로](#운영체제)