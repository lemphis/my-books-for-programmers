# 84. 프로그램의 동작을 스레드 스케줄러에 기대지 말라

여러 스레드가 실행 중이면 운영체제의 스레드 스케줄러가 어떤 스레드를 얼마나 오래 실행할지 정한다.  
정상적인 운영체제라면 이 작업을 공정하게 수행하지만 구체적인 스케줄링 정책은 운영체제마다 다를 수 있다.  
따라서 잘 작성된 프로그램이라면 이 정책에 좌지우지되어서는 안 된다.  
**정확성이나 성능이 스레드 스케줄러에 따라 달라지는 프로그램이라면 다른 플랫폼에 이식하기 어렵다.**

견고하고 빠릿하고 이식성 좋은 프로그램을 작성하는 가장 좋은 방법은 실행 가능한 스레드의 평균적인 수를 프로세서 수보다 지나치게 많아지지 않도록 하는 것이다.  
실행 가능한 스레드 수를 적게 유지하는 주요 기법은 각 스레드가 무언가 유용한 작업을 완료한 후에는 다음 일거리가 생길 때까지 대기하도록 하는 것이다.  
**스레드는 당장 처리해야 할 작업이 없다면 실행돼서는 안 된다.**
