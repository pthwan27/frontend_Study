# Next.js
## Q1. Next.js에서 데이터를 가져오는 방법은 뭐가 있을까요??
데이터를 가져오는 방법은 여러가지가 있지만 그 중 대표적으로
getServerSideProps, getStaticProps, getStaticPaths가 있습니다.

## Q2. getServerSideProps와 getStaticProps는 언제 사용하는게 좋을까요?
**getServerSideProps**는 사용자 프로필 페이지와 같이 사용자 별 다른 내용을 뿌려줘야하는 경우 사용하면 좋습니다.
**getStaticProps**는 블로그 글 목록과 같이 자주 변경되지 않는 곳에서 사용하면 좋습니다.
