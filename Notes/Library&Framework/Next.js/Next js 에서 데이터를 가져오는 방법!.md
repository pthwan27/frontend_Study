# **Next.js 에서 데이터를 가져오는 방법!**

Next에서 데이터를 가져오는 방법은 여러가지가 있습니다.

<br/>

# 1. **getServerSideProps(SSR)**

→ 서버에서만 실행되고, 브라우저에서는 실행되지 않기 때문에 브라우저에 로그가 찍히지 않습니다.

```jsx
/*
    context object 에 있는 데이터
    
    params: 페이지가 동적 경로일 경우 - 페이지 이름이 [id].js일 경우 params는 { id: 값 }
    req: HTTP request object
    res: HTTP response object
    query: 동적 경로 매개변수를 포함한 query의 값
    preview: 페이지가 Preview Mode일 경우 true
    previewData: setPreviewData에서 설정한 데이터
    등등...
*/

export async function getServerSideProps(context) {
  return {
    props: {}, // 컴포넌트가 받을 props
    revalidate: 0, // 페이지가 재생성될 지연 시간
    notFound: false, // true일 경우 404
    redirect: {
      // 리다이렉트할 페이지의 경로
      destination: "/", // 리다이렉트 경로
    },
  };
}
```

> pre-render란 빌드할 때 특정 페이지를 미리 HTML을 만들어주는 기능

<br/>

**getServerSideProps**는 페이지만 사용 가능합니다. 페이지가 아닌 파일
(\_app, \_document)에서는 사용할 수 없습니다.

**getServerSideProps**는 서버에서만 실행되어서 브라우저에서 로그를 확인할 수 없습니다.

**getServerSideProps**가 선언된 페이지는 빌드와 상관없이 매번 페이지에 들어올 때마다 데이터를 서버에 요청합니다. 그리고 반환한 props를 이용해서 렌더링 합니다.
\
<br/>

## **사용해야할 때**

→ 요청할 때 데이터를 가져와야하는 페이지를 pre-render해야할 때 사용합니다!

→ 데이터가 많이 바뀔 때 사용

<br/>

## **getServerSideProps , getStaticProps**의 사용 예시

**getServerSideProps**는 사용자 프로필 페이지와 같이 사용자 별 다른 내용을 뿌려줘야하는 경우 사용하면 좋습니다.

```jsx
// pages/profile/[id].js

import axios from "axios";

function Profile({ user }) {
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}

export async function getServerSideProps(context) {
  const { id } = context.params;
  const response = await axios.get(`https://api.example.com/users/${id}`);
  const user = response.data;
  return {
    props: {
      user,
    },
  };
}

export default Profile;
```

**getStaticProps**는 블로그 글 목록과 같이 자주 변경되지 않는 곳에서 사용하면 좋습니다.

```jsx
// pages/posts.js

import axios from "axios";

function Posts({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}

export async function getStaticProps() {
  const response = await axios.get("https://api.example.com/posts");
  const posts = response.data;
  return {
    props: {
      posts,
    },
    revalidate: 60, // 1분마다 캐시 갱신
  };
}

export default Posts;
```

<br/>

# 2. **getStaticProps - SSG(Static Site Generation)**

![사용예시](../../../images/Library&Framework/Next.js/Next%20js%20%EC%97%90%EC%84%9C%20%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%A5%BC%20%EA%B0%80%EC%A0%B8%EC%98%A4%EB%8A%94%20%EB%B0%A9%EB%B2%95!/getStaticProps.png)

<br/>

```jsx
/*
    context object 에 있는 데이터
    
    params: 페이지가 동적 경로일 경우 - 페이지 이름이 [id].js일 경우 params는 { id: 값 }
    req: HTTP request object
    res: HTTP response object
    query: 동적 경로 매개변수를 포함한 query의 값
    preview: 페이지가 Preview Mode일 경우 true
    previewData: setPreviewData에서 설정한 데이터
*/

export async function getStaticProps(context) {
  return {
    props: {}, // 컴포넌트가 받을 props
    revalidate: 10, // 페이지가 재생성될 지연 시간
    notFound: false, // true일 경우 404
    redirect: {
      // 리다이렉트할 페이지의 경로
      destination: "/", // 리다이렉트 경로
    },
  };
}
```

**getStaticProps**은 SSG 으로 만들기 위해 사용하며,프로젝트가 빌드될 때 데이터를 불러옵니다.(SEO에 유리)

**getStaticProps**는 페이지에서만 사용 가능합니다, 페이지가 아닌 파일에서는 사용할 수 없습니다(\_app, \_document 등)

**getStaticProps**는 매번 데이터를 요청하지 않기 때문에 업데이트가 잦지 않은 static한 페이지일 경우 추천합니다.

## **사용해야 할 때**

→ 사용자의 요청보다 먼저 build 시간에 필요한 데이터를 가져올 때

→ 데이터를 공개적으로 캐시할 수 있을 때

→ 페이지는 미리 렌더링되어야 하고(SEO의 경우) 매우 빨라야 할 때

<br/>

# 3. **getStaticPaths**

```jsx
// pages/posts/[id].js

export async function getStaticPaths() {
	const res = await fetch(...);
    const posts = await res.json();

    const paths = posts.map((post)) => ({
    	params: { id: post.id }
    });
    return {
	paths,			// 빌드타임에 pre-render 할 경로들
        fallback: false,	// ture or 'blocking'   -> 자세한건 아래에서 설명
    }
}

// `getStaticPaths` requires using `getStaticProps`
export async function getStaticProps(context) {
  return {
    // Passed to the page component as props
    props: { post: {} },
  }
}

// router.isFallback를 이용해서 Fallback이 렌더링되고 있는지 감지
export default function Post({ post }) {
  const router = useRouter()

  // If the page is not yet generated, this will be displayed
  // initially until getStaticProps() finishes running
  if (router.isFallback) {
    return <div>Loading...</div>
  }

  // Render post...
}
```

</br>

**getStaticPaths** 는 동적 경로를 이용해서 페이지를 만들고 싶은데 SSG를 사용하고 싶을 때 getStaticProps 를 같이 사용하면서 빌드 타임 때 정적으로 렌더링 할 path를 설정해줍니다.

**getStaticPaths** 는 **getServerSideProps** 와 사용할 수 없습니다. 무조건 **getStaticProps** 와 함께 사용해야 합니다.

**getStaticPaths** 는 paths와 fallback을 반환하는데, paths는 빌드 타임에 pre-render 할 경로들이고, fallback은 false, true, 'blocking'를 반환합니다.

## **paths**

paths는 어떤 경로가 pre-render될지 결정합니다.

→ 만약 pages/posts[id].js 이라는 이름의 동적 라우팅을 사용하는 페이지가 있다면 아래와 같이 사용합니다. 그러면 빌드하는 동안 /posts/1 , /posts/2를 생성하게 됩니다!!

![Untitled](../../../images/Library&Framework/Next.js/Next%20js%20%EC%97%90%EC%84%9C%20%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%A5%BC%20%EA%B0%80%EC%A0%B8%EC%98%A4%EB%8A%94%20%EB%B0%A9%EB%B2%95!/paths.png)

## **params**

페이지 이름이 pages/posts/[postId]/[commentId]라면, params는 postId와 commentId입니다~

만약 페이지 이름이 pages/[…slug] 와 같이 모든 경로를 사용한다면 , params는 slug가 담인 배열이여야 합니다!!

## **fallback**

**false** 인 경우 paths에 등록되지 않은 경로로 들어가면 404 페이지가 됩니다. 이 옵션은 생성할 path가 적거나 새로운 페이지가 자주 추가되지 않는 경우에 유용합니다.

**true** 인 경우 paths에 등록되지 않은 경로로 들어가면 404 페이지가 되는 대신에 별도의 컴포넌트를 보여줄 수 있습니다.(router.isFallback으로 감지) 그 후 getStaticProps 를 통해 데이터를 가져오고 props를 가져오면 정적 페이지를 생성합니다. 당시에는 pre-render가 되지 않지만 한번 렌더링 후부터는 pre-render에 포함됩니다. (와디즈의 펀딩처럼 계속적으로 상품이 등록될 때 유용한 기능입니다.)

**blocking** 인 경우 paths에 포함되지 않은 경로는 SSR과 동일하게 생성합니다. getStaticPaths 가 될 때까지 기다린 다음 성공하면 해당 페이지 경로를 캐싱한 후 다음 요청 시에는 캐싱된 데이터를 리턴합니다. true와 동일하게 한번 렌더링 후부터는 pre-render가 됩니다.
