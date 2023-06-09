## async, sync & blocking , non-blocking

### <span style="color:red;  font-weight:bold"> Q. </span> 동기, 비동기의 차이가 뭔지 아시나요 ?

<span style="color:#00ff00;  font-weight:bold"> A. </span> <br/>
**작업에 대한 완료 여부** 를 신경쓰는지 안쓰는지의 차이로 알고 있습니다.
작업을 순차적으로 실행한다면 동기, 그게 아니라면 비동기라고 할 수 있습니다.

### <span style="color:red;  font-weight:bold"> Q. </span> 블로킹과 논블로킹의 차이가 뭔지 아시나요?

<span style="color:#00ff00;  font-weight:bold"> A. </span> <br/> 블로킹, 논블로킹은 흐름 자체를 막는지 안막는지의 차이로 알고 있습니다.

<span style="color:#00ff00;  font-weight:bold"> A. </span> <br/>
A와 B함수가 있을 때
A가 실행 중 B함수를 호출했을 때 A를 멈추고 B를 실행하느냐 A를 계속 실행하며 B를 실행하는지 차이가 Blocking과 Non-Blocking의 차이라고 생각합니다.

<span style="color:#00ff00;  font-weight:bold"> A. </span><br/>
A라는 함수가 제어권을 가지고 있을 때 B함수를 호출한다면 B를 호출만 뒤에도 A를 계속해서 실행하는 것이고 이것을 Non-Blocking이라고 하고

B의 함수에게 제어권을 넘겨준다면 B의 함수를 완료하고 다시 A를 실행하게 되는데 이를 Blocking이라고 합니다.

### <span style="color:red;  font-weight:bold"> Q. </span> 그렇다면 비동기와 논블로킹의 차이점은 뭔가요?

<span style="color:#00ff00;  font-weight:bold"> A. </span></br>
비동기와 논블로킹의 차이는 **작업 수행 방식의 차이**라고 생각합니다.

비동기는 작업의 완료 여부를 신경쓰지 않고 여러가지 작업을 동시에 수행하는 것이고
논블로킹은 다른 함수를 호출하여 수행할 때 현재 함수를 멈추지 않고 계속 실행하는 것으로 알고 있습니다.

### <span style="color:red;  font-weight:bold"> Q. </span> 블로킹, 논블로킹, 동기, 비동기 장단점

<span style="color:#00ff00;  font-weight:bold"> A. <br/> </span>블로킹 <br/>

> 장점<br/>
> 작업이 순차적으로 이루어지기에 작업 흐름을 쉽게 이해할 수 있음

> 단점<br/>
> 블로킹이 이루어지는 동안엔 하드웨어 리소스를 효율적으로 이용하지 못함

논 블로킹 <br/>

> 장점<br/>
> 리소스가 낭비되는 시간이 없기에 하드웨어 리소스를 균일하고 효율적으로 이용가능

> 단점<br/>
> 업무 흐름이 매우 복잡해지는 단점 존재

동기 <br/>

> 장점<br/>
> 요청한 작업의 완료여부와 결과를 바로 알 수 있다는 장점이 있음

> 단점<br/>
> 요청한 작업의 완료여부와 작업 결과를 반환받는 데 필요한 시간이 긴 경우 문제가 발생

비동기 <br/>

> 장점<br/>
> 이미 요청한 작업의 결과를 기다리고 있을 필요 없이 바로 다음 작업을 요청할 수 있기에 작업 효율이 높아짐(자원의 효율적 사용)

> 단점 <br/>
> 특정 작업의 경우 선행 작업의 결과값을 이어받아 순차적으로 작업을 진행해야 할 수 있다. 이런 작업을 비동기 작업으로 구현할 경우 프로그램이 복잡해짐
