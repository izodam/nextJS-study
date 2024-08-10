이용할 API : https://nomad-movies.nomadcoders.workers.dev/

영화 목록을 불러옵니다.

```
/: This page
/movies: List popular movies
/movies/:id: Get movie by :id
/movies/:id/credits: Get credits for a movie by :id
/movies/:id/videos: Get videos for a movie by :id
/movies/:id/providers: Get providers for a movie by :id
/movies/:id/similar: Get similar movies for a movie by :id
```

## Client side

Client side에서 API요청을 보내는 경우...

```ts
"use client";

import { useEffect, useState } from "react";

export default function Home() {
  const [isLoading, setIsLoading] = useState(true);
  const [movies, setMovies] = useState([]);
  const getMovies = async () => {
    const response = await fetch(
      "https://nomad-movies.nomadcoders.workers.dev/movies"
    );

    const json = await response.json();

    setMovies(json);
    setIsLoading(false);
  };

  useEffect(() => {
    getMovies();
  }, []);

  return <div>{isLoading ? "Loading..." : JSON.stringify(movies)}</div>;
}
```

좋지 않음....!!

## Server side

server side를 이용하면

```ts
export const metadata = {
  title: "Home",
};
const URL = "https://nomad-movies.nomadcoders.workers.dev/movies";

async function getMovies() {
  const response = await fetch(URL);
  const json = await response.json();
  return json;
}

export default async function HomePage() {
  const movies = await getMovies();
  return <div>{JSON.stringify(movies)}</div>;
}
```

만약 데이터 fetch가 오래걸린다면 사용자는 아무 화면도 확인하지 못한채 기다려여야 한다.

## Loading Components

loading.tsx를 만들면, 로딩중에는 해당 페이지를 보여주게 된다.

## Parallel request

만약 2개의 data를 fetch 해야할 때

```ts
import { API_URL } from "../../../(home)/page";

async function getMovie(id: string) {
  const response = await fetch(`${API_URL}/${id}`);
  return response.json();
}

async function getVideos(id: string) {
  const response = await fetch(`${API_URL}/${id}/videos`);
  return response.json();
}

export default async function MovieDetail({
  params: { id },
}: {
  params: { id: string };
}) {
  const movie = await getMovie(id);
  const vidios = await getVideos(id);
  return <h1>{movie.title}</h1>;
}
```

이런식으로 한다면 getVideos라는 함수는 getMovie가 다 된 후에 작동하므로, 시간이 오래걸린다

-> Promise.all을 하면 getMovie와 getVideos를 동시에 처리!!

```ts
async function getMovie(id: string) {
  const response = await fetch(`${API_URL}/${id}`);
  return response.json();
}

async function getVideos(id: string) {
  const response = await fetch(`${API_URL}/${id}/videos`);
  return response.json();
}

export default async function MovieDetail({
  params: { id },
}: {
  params: { id: string };
}) {
  const [movie, videos] = await Promise.all([getMovie(id), getVideos(id)]);

  return <h1>{movie.title}</h1>;
}
```

이런식으로 해도, 페이지는 movie와 video 둘 다 끝나기를 기다렸다가 보여줌.

그게 아니라 하나만 연결되어도 그 정보 먼저 띄워주고 싶음.

## suspense

먼저 data를 fetch한 요소를 먼저 띄워줌 -> react

```ts
import { Suspense } from "react";
import MovieInfo from "../../../../components/movie-info";
import MovieVideos from "../../../../components/movie-videos";

export default async function MovieDetail({
  params: { id },
}: {
  params: { id: string };
}) {
  return (
    <div>
      <Suspense fallback={<h1>Loading movie info</h1>}>
        <MovieInfo id={id} />
      </Suspense>
      <Suspense fallback={<h1>Loading movie videos</h1>}>
        <MovieVideos id={id} />
      </Suspense>
    </div>
  );
}
```
