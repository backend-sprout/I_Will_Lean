# JVM 이란?     
                              
자바 바이트 코드를 실행해주는 버츄얼 머신으로                  
JVM을 사용하면 바이트 코드를 현재 OS 환경에 맞추어 기계어로 변역하고 실행해준다.             

# JVM 흐름 

1. 자바 파일을 작성
2. 자바 컴파일러를 통해서 바이트 코드인 클래스파일로 생성 
3. Class Loader를 통해 class 파일들을 JVM으로 로딩한다.
4. 로딩된 class 파일들은 Execution engine을 통해 해석된다.(인터프리터로 올리고 JIT compiler)
5. 해석된 바이트코드는 Runtime Data Areas에 배치되어 수행이 이루어진다.
6. 실행 과정 속에서 JVM은 필요에 따라 Thread Synchronization과 GC같은 관리작업을 수행한다.

런타임 데이터 올리기전에 -> 









