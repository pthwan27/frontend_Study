# CORS

## Q1. CORS란?

**한 출처**에서 실행 중인 웹 애플리케이션이 **다른 출처**의 선택한 **자원**에 **접근할 수 있도록** 권한을 부여하도록 **브라우저에 알려주는 체제**입니다.

## Q2. SOP가 필요한 이유?

해커가 의도적으로 로그인된 사용자에게 악성 스크립트를 실행시키는 주소를 보내고 로그인된 사용자의 인증토큰을 이용해 공격을 시도합니다.
이 때 공격이 어느 출처에서 왔는지 확인하는 과정에서 공격의 출처가 SOP를 위반하여 공격이 실패합니다.
이처럼 의도하지 않은 출처에서 오는 요청을 막기 위해 SOP를 사용합니다.

## Q3. Preflight가 필요한 이유

만약 Preflight가 없을 때 우리가 요청을 Browser에 보내면 브라우저는 그대로 서버에 보냅니다!

만약 처음 요청이 DB에 모든 데이터를 삭제하는 것이다?
그렇다면 우리의 DB 데이터를 모두 삭제한 뒤에 CORS 오류가 발생할 것입니다.

따라서 CORS를 모르는 서버를 위해 Preflight를 통한 사전 확인 작업을 통해 Server에 요청이 가능한지 확인한 후 본 요청을 해야합니다.