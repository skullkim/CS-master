# 운영체제란
운영체제는 사용자가 애플리케이션을 보다 쉽게 사용할 수 있게 도움을 주는 역할을 합니다. 다양한 자원관리를 통해 프로세스들이 concurrent 또는 parallel하게 실행되게 해줍니다. 또 한, 다양한 하드웨어 기기들이 서로 상호작용할 수 있게 해줍니다. 운영체제가 하는 역할들은 다음과 같습니다.
- program execution
    - 시스템은 프로그램을 램에 로딩하고 실행할 수 있어야 합니다.
- I/O operation
    - 실행중인 프로그램은 IO 디바이스 또는 파일로부터 IO를 해야 합니다. 유저가 직접 IO 디바이스를 컨트롤할 수 없으므로 운영체제는 IO operation을 제공해야 합니다.
- error detection
    - 에러가 발생하면 OS는 해당 에러를 감지하고 수정해 컴퓨팅을 지속할 수 있어야 합니다.
- protection and security
    - 시스템 리소스에 대한 모든 접근은 올바른 제어 하에 이루어져야 합니다.
    - IO 디바이스가 잘못된 접근을 하는 것을 막기 위해 유저 인증을 거쳐야 합니다.
- logging
    - 어떤 프로그램이 어떤 리소스를 얼만큼 사용했는지를 추적해야 합니다.
- File-system manipulation
    - 프로그램은 파일이나 디렉터리에 대해 CRUD를 해야 합니다. 접근 권한 관리도 해야 합니다.
- provide user interface
    - 운영체제는 유저 인터페이스를 제공해 유저가 편리하게 OS를 사용할 수 있게 해야 합니다.
- communication
    - 같은 PC 또는 서로 다른 PC에 있는 프로세스끼리 정보를 교환해야할 경우 이 기능을 제공해야 합니다.
- resource allocation
    - 멀티 프로세스들이 concurrent하게 동작할때, OS는 각 프로세스들에게 적절한 리소스를 할당해주어야 합니다.
# 프로세스 vs 스레드
프로세스와 스레드는 모두 작업 단위입니다. OS 입장에서 가장 작은 작업 단위는 프로세스이고 CPU 입장에서 가장 작은 작업 단위는 스레드입니다. 
프로세스는 저장장치에 있던 프로그램이 램에 올라와 실행되는 것을 의미하며, 스레드는 프로세스 내부의 하나의 작업 흐름입니다. 프로세스 생성을 위해 램에 해당 프로세스를 위한 공간이 할당되면 이 공간은 크게 4가지 구조로 나뉩니다. 이 4가지 구조는 각각 code section, data section, heap section, stack section입니다. 또 한, 프로세스 하나에 대한 정보는 PCB라는 블록에 저장됩니다.
스레드의 경우 프로세스 내부에 존재하기 때문에 프로세스가 가진 code section, data section, resource를 스레드끼리 공유합니다. 또 한, 유저 스레드가 실행되기 위해선 커널 스레드와 매칭되야 합니다. 유저 스레드와 커널 스레드를 매칭시키는 방법은 다양하지만 리눅스는 one-to-one 방식을 사용합니다. 하드웨어 스레드의 경우 CPU가 작업을 실행하면서 메모리에 접근해 IO를 할 때 코어가 아무런 일도 할 수 없다는 문제를 해결하기 위해 등장했습니다. 하드웨어 스레드는 각자 하나의 테스크를 잡고 있습니다. 코어가 하나의 스레드를 실행하면서 CPU 연산이 끝나고 메모리에 접근해 IO를 하면, 해당 코어는 또 다른 스레드를 실행해 연산을 합니다. 이런식으로 코어가 여러 하드웨어 스레드를 번갈아가면서 실행한다면  공백을 줄일 수 있습니다.
# 프로세스 주소 공간
프로세스 주소 공간은 크게 4개 구조로 나누어져 있습니다.
- data section
    - 전역 변수나 static 변수등이 존재합니다
- code section
    - 프로그램 실행에 필요한 기계어 코드가 존재합니다. 이 부분은 read only입니다
- heap section
    - 런타임때 동적할당된 메모리영역입니다
- stack section
    - 함수 호출 시 함수를 관리하기 위해 사용하는 공간입니다. 지역 변수가 이 영역에서 관리됩니다
# 인터럽트(Interrupt)
인터럽트는 CPU에게 특정 작업이 끝났음을 알리는 일종의 신호입니다. 관점에 따라 인터럽트는 여러 분류로 분류할 수 있습니다. 소프트웨어 인터럽트와 하드웨어 인터럽트. 또는 maskable interrucpt와 unmaksable interrupt로도 나눌 수 있습니다. 인터럽트가 발생하면 인터럽트 종류를 나타내는 interrupt number를 interrupt request line으로 보냅니다. CPU는 하나의 instruction을 끝낼때마다 이 interrupt request line에 접근해 interrupt 발생 여부를 조사합니다. interrupt가 발생했다면 iterrupt vector에 접근해 interrutp number를 키값으로 활용해 interrupt에 대한 작업을 수행할 수 있는 interrupt handler routine을 얻고 이를 실행합니다. 이 때, 기존에 실행중이던 작업은 context switching를 ready 상태로 바꾸어야 합니다. 이 작업을 interrupt handler가 실행합니다.
# 시스템 콜(System Call)
시스템콜은 유저 모드 애플리케이션이 운영체제가 제공하는 기능들을 사용할 수 있게 제공되는 인터페이스입니다. 프로그램은 직접적으로 시스템콜을 사용하는 대신 API를 사용해 시스템콜 서비스를 사용합니다. POSIX API가 시스템콜을 사용할 수 있게 해주는 API 예시입니다.
시스템 콜 종류는 다음과 같습니다
- processsss control
    - 프로세스 생성, 종료, 이벤트 대기
    - 메모리 할당, 해제
    - 프로세스간 공유 데이터에 대한 락 관리 
- file management
    - 파일 생성, 해제
    - 읽고 쓰기
- device management
    - 디바이스에 대한 관리
    - 디바이스 버퍼에 대한 읽고 쓰기
- information maintenance
    - 시스템 데이터에 대한 접근
    - 타임에 대한 접근
- communication
    - 프로세스간 통신
- protection
    - 리소스에 대한 접근 관리
    - 리소스 접근 권한 관리
# PCB와 Context Switching
PCB는 process control block으로, 프로세스 하나에 대한 정보를 보관합니다. PCB 내부에는 프로세스를 식별할 수 있는 ID값, program vounter, CPU scheduling information(priority, scheduling queue pointer), I/O status information 등이 존재합니다.
context switching은 ready 상태에 있는 process에게 CPU resource를 할당하고, running 상태에 있는 프로세스를 다시 ready 상태로 돌려놓을 때 발생합니다. process가 running 상태에서 CPU 자원을 사용할 때 CPU와 메모리 속도 차이를 극복하기 위해 중간에 레지스터를 이용해 데이터와 실행할 instruction들을 저장합니다. 따라서 CPU에 다른 프로세스를 할당할려면 레지스터에 있던 값들을 PCB에 저장하고, instruction을 어디까지 실행했는지도 PCB에 저장해야 하는대 context switching이 이 과정입니다.

# IPC(Inter Process Communication)
IPC는 프로세스간에 데이터를 공유하는 방법입니다. 프로세스들은 단순한 데이터 공유, 연산 속도 증가, modularity를 위해 IPC를 사용합니다.
IPC를 구현한 방식으로는 shared memory, message passing이 있습니다. 
- shared memory는 하나의 물리적 메모리에서 여러 프로세스들이 서로 공유 가능한 메모리 공간을 할당해 사용하는 방식입니다. 따라서 한대의 물리적 컴퓨터에서 여러 프로세스가 IPC를 할때 사용됩니다. 여러 프로세스가 함부로 shared memory에 접근해 데이터를 수정한다면 racing condition, dead lock 등의 상황이 발생할 수 있으므로 동기화를 해주어야 합니다.
- message passing은 분산 컴퓨팅 환경 등 shared memory를 사용할 수 없는 환경에서 사용합니다. message passing은 OS가 message queue라는 공간을 메모리에 할당하고 관리합니다. 그러면 process는 필요한 메시지를 전송하고 이 메시지는 message queue에 위치하게 됩니다. 
# CPU 스케줄링
CPU 스케줄링은 여러개의 process나 스레드들이 있을 때, 이들에게 CPU를 자원을 어떻게 할당할지에 대한 문제를 다룹니다. CPU는 한 번에 하나의 일만 처리할 수 있으므로 CPU 스케줄링을 통해 CPU 자원을 최대한 공평하게 나눌 필요가 있습니다.

CPU 스케줄링을 하는 여러 알고리즘이 존재합니다. FCFS, SJF, round robine은 하나의 ready 큐에 존재하는 프로세스들을 스케줄링 할지를 결정하는 알고리즘종류입니다. 여러개의 큐를 사용하는 multilevel queue scheduling과 multilevel feedback queue scheduling도 존재합니다.
프로세스가 아닌 스레드 간의 스케줄링의 경우 OS가 사용하는 유저 스레드와 커널 스레드 매핑 방식에 따라 process-contention scope과 system-contention scope으로 나뉩니다. process-contention scope은 many-to-one과 many-to-many 모델에서 스레드 라이브러리가 user-level thread를 사용 가능한 LWP에 어떻게 스케줄링 할지를 결정합니다. system-contention scope은 one-to-one model에서 kernel-level thread를 사용 가능한 CPU에 어떻게 할당할지를 결정합니다.

임베디스용 운영체제인 real-time os의 경우 CPU 스케줄링 방법으로 priority-based sceduling, rate monotonic scheduling, earliest deadline first scheduling 등이 존재합니다.
# 데드락(DeadLock)
데드락은 두 개 이상의 프로세스, 스레드가 필요한 자원을 얻지 못해 무한 대기 상태에 빠지는 것을 의미합니다. 자원을 공유할 때, 해당 자원에 대한 올바른 동기화를 지원하지 못해 발생합니다. 예를 들어 담은과 같은 코드에서 P1과 P2가 각각 lock1=true, lock2=true를 실행한 뒤 burst가 되었다면 데드락으로 인해 무한루프에 빠지게됩니다.
```
#include <stdio.h>
 
// C 에는 boolean이 없어 직접 정의한다.
typedef enum {false, true} boolean;
extern boolean lock1 = false;
extern boolean lock2 = false;
extern int balance;
 
// process P1
int main() {
    lock1 = true;
    while(lock2 == true);
    balance += 10;
    lock1 = false;
}
 
// process P2
int main() {
    lock2 = true;
    while(lock1 == true);
    balance += 10;
    lock2 = false;
}

```
# Race Condition
race condition은 서로 다른 프로세스 또는 스레드가 공유 자원에 접근하는 순서에 따라 실행 결과가 달라지는 것을 의미합니다. 이는 공유 자원에 대해 올바른 동기화를 하지 못했을 때 발생하는 문제 중 하나입니다.
예를 들어 다음과 같은 코드에서 첫 번째 프로세스 P1이 while문을 끝내고 burst가 되었다면 P2는 lock이 false이므로 임계구역에 진입할 수 있게 됩니다. 그러면 두 개의 프로세스 모두 임계구역에 진입하기 때문에 race condition이 발생합니다.
```
#include <stdio.h>
 
// C 에는 boolean이 없어 직접 정의한다.
typedef enum {false, true} boolean;
extern boolean lock = false;
extern int balance;
 
int main() {
    while(lock == true);
    lock = true;
    balance += 10;
    lock = false;
}

```
# 세마포어(Semaphore) & 뮤텍스(Mutex)

# 페이징 & 세그먼테이션 (PDF)

# 페이지 교체 알고리즘

# 메모리(Memory)
메모리는 1차 메모리와 2차 메모리로 나누어져 있습니다. 1차 메모리는 RAM이라 불리며 CPU가 유일하게 접근할 수 있는 대용량 메모리입니다. 휘발성이라는 특징을 가지고 있습니다. 2차 메모리는 1차 메모리에 비해 용량이 크지만 느리다는 문제가 있습니다. 1차 메모리와 달리 휘발하지 않습니다. 통상적으로 OS가 2차 메모리에 접근할 때는 큰 용량의 데이터를 읽고 써야 합니다. 따라서 이 경우 interrupt를 통해 데이터를 1차 메모리에 올리는 대신 DMA를 사용합니다.
# 파일 시스템

