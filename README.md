---
layout: home
title: 박민서 블로그
permalink: /
---

<html>
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://cdn.tailwindcss.com?plugins=forms,typography"></script>
		<script src="https://unpkg.com/unlazy@0.11.3/dist/unlazy.with-hashing.iife.js" defer init></script>
		<script type="text/javascript">
			window.tailwind.config = {
				darkMode: ['class'],
				theme: {
					extend: {
						colors: {
							border: 'hsl(var(--border))',
							input: 'hsl(var(--input))',
							ring: 'hsl(var(--ring))',
							background: 'hsl(var(--background))',
							foreground: 'hsl(var(--foreground))',
							primary: {
								DEFAULT: 'hsl(var(--primary))',
								foreground: 'hsl(var(--primary-foreground))'
							},
							secondary: {
								DEFAULT: 'hsl(var(--secondary))',
								foreground: 'hsl(var(--secondary-foreground))'
							},
							destructive: {
								DEFAULT: 'hsl(var(--destructive))',
								foreground: 'hsl(var(--destructive-foreground))'
							},
							muted: {
								DEFAULT: 'hsl(var(--muted))',
								foreground: 'hsl(var(--muted-foreground))'
							},
							accent: {
								DEFAULT: 'hsl(var(--accent))',
								foreground: 'hsl(var(--accent-foreground))'
							},
							popover: {
								DEFAULT: 'hsl(var(--popover))',
								foreground: 'hsl(var(--popover-foreground))'
							},
							card: {
								DEFAULT: 'hsl(var(--card))',
								foreground: 'hsl(var(--card-foreground))'
							},
						},
					}
				}
			}
		</script>
		<style type="text/tailwindcss">
			@layer base {
				:root {
					--background: 0 0% 100%;
--foreground: 240 10% 3.9%;
--card: 0 0% 100%;
--card-foreground: 240 10% 3.9%;
--popover: 0 0% 100%;
--popover-foreground: 240 10% 3.9%;
--primary: 240 5.9% 10%;
--primary-foreground: 0 0% 98%;
--secondary: 240 4.8% 95.9%;
--secondary-foreground: 240 5.9% 10%;
--muted: 240 4.8% 95.9%;
--muted-foreground: 240 3.8% 46.1%;
--accent: 240 4.8% 95.9%;
--accent-foreground: 240 5.9% 10%;
--destructive: 0 84.2% 60.2%;
--destructive-foreground: 0 0% 98%;
--border: 240 5.9% 90%;
--input: 240 5.9% 90%;
--ring: 240 5.9% 10%;
--radius: 0.5rem;
				}
				.dark {
					--background: 240 10% 3.9%;
--foreground: 0 0% 98%;
--card: 240 10% 3.9%;
--card-foreground: 0 0% 98%;
--popover: 240 10% 3.9%;
--popover-foreground: 0 0% 98%;
--primary: 0 0% 98%;
--primary-foreground: 240 5.9% 10%;
--secondary: 240 3.7% 15.9%;
--secondary-foreground: 0 0% 98%;
--muted: 240 3.7% 15.9%;
--muted-foreground: 240 5% 64.9%;
--accent: 240 3.7% 15.9%;
--accent-foreground: 0 0% 98%;
--destructive: 0 62.8% 30.6%;
--destructive-foreground: 0 0% 98%;
--border: 240 3.7% 15.9%;
--input: 240 3.7% 15.9%;
--ring: 240 4.9% 83.9%;
				}
			}
		</style>
  </head>
  <body>
    <div class="min-h-screen flex flex-col bg-background text-foreground">
  <header class="p-6 bg-primary text-primary-foreground">
    <h1 class="text-3xl font-bold">박민서의 블로그</h1>
    <p class="mt-2 text-muted-foreground">생각을 나누고, 지식을 공유하는 공간입니다.</p>
  </header>
  <main class="flex-grow p-6">
    <section class="mb-8">
      <h2 class="text-2xl font-semibold mb-4">환영합니다!</h2>
      <p class="text-muted-foreground">
        이 블로그는 박민서가 자신의 생각과 경험을 기록하고, 독자들과 함께 나누기 위해 만든 공간입니다.
        다양한 주제에 대해 글을 올리고, 여러분의 피드백과 관심을 기다리고 있습니다.
      </p>
    </section>
    <section class="mb-8">
      <h2 class="text-2xl font-semibold mb-4">내 포트폴리오</h2>
      <div class="bg-card p-6 rounded-lg shadow-sm transition-all duration-300 hover:shadow-md">
        <h3 class="text-lg font-medium mb-4">박민서</h3>
        <p class="text-card-foreground mb-4">
          저는 박민서입니다. 프론트엔드 개발자이자 디자이너로, 웹을 통해 창의적인 아이디어를 구현하고
          사람들과 소통하는 것을 좋아합니다. 이 블로그는 제 기술, 일상, 문화, 여행 등 다양한 관심사와 경험을
          기록하고 공유하기 위한 공간입니다.
        </p>
        <div class="flex flex-wrap gap-3 mb-4">
          <a href="#" class="bg-secondary text-secondary-foreground px-4 py-2 rounded hover:bg-secondary/80 transition-colors">
            GitHub
          </a>
          <a href="#" class="bg-secondary text-secondary-foreground px-4 py-2 rounded hover:bg-secondary/80 transition-colors">
            LinkedIn
          </a>
          <a href="#" class="bg-secondary text-secondary-foreground px-4 py-2 rounded hover:bg-secondary/80 transition-colors">
            포트폴리오 사이트
          </a>
        </div>
        <img
          aria-hidden="true"
          alt="portfolio-image"
          src="https://picsum.photos/seed/portfolio/400/250"
          class="w-full h-40 object-cover rounded-md"
        />
      </div>
    </section>
    <section class="mb-8">
      <h2 class="text-2xl font-semibold mb-4">최신 글</h2>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div class="bg-card p-4 rounded-lg shadow-sm transition-all duration-300 hover:shadow-md">
          <img
            aria-hidden="true"
            alt="blog-post-1"
            src="https://picsum.photos/seed/blog1/400/250"
            class="w-full h-40 object-cover rounded-md mb-4"
          />
          <h3 class="text-lg font-medium mb-2">제목 1: 생각의 시작</h3>
          <p class="text-muted-foreground mb-2">2023년 10월 10일</p>
          <p class="text-card-foreground">
            이 글에서는 새로운 아이디어를 어떻게 탄생시키는지에 대해 이야기해보겠습니다.
          </p>
        </div>
        <div class="bg-card p-4 rounded-lg shadow-sm transition-all duration-300 hover:shadow-md">
          <img
            aria-hidden="true"
            alt="blog-post-2"
            src="https://picsum.photos/seed/blog2/400/250"
            class="w-full h-40 object-cover rounded-md mb-4"
          />
          <h3 class="text-lg font-medium mb-2">제목 2: 경험을 나누다</h3>
          <p class="text-muted-foreground mb-2">2023년 10월 5일</p>
          <p class="text-card-foreground">
            실제 경험을 바탕으로 한 이야기와 배움을 공유해보려 합니다.
          </p>
        </div>
      </div>
    </section>
    <section class="mb-8">
      <h2 class="text-2xl font-semibold mb-4">소개</h2>
      <div class="bg-card p-6 rounded-lg shadow-sm transition-all duration-300 hover:shadow-md">
        <p class="text-card-foreground">
          저는 박민서입니다. 이 블로그는 제 삶의 다양한 경험, 생각, 그리고 배움을 기록하고
          공유하기 위해 시작했습니다. 기술, 문화, 일상, 그리고 더 많은 주제들을 다루고자 합니다.
        </p>
      </div>
    </section>
    <section>
      <h2 class="text-2xl font-semibold mb-4">카테고리</h2>
      <div class="flex flex-wrap gap-3">
        <button class="bg-secondary text-secondary-foreground px-4 py-2 rounded hover:bg-secondary/80 transition-colors">
          기술
        </button>
        <button class="bg-secondary text-secondary-foreground px-4 py-2 rounded hover:bg-secondary/80 transition-colors">
          일상
        </button>
        <button class="bg-secondary text-secondary-foreground px-4 py-2 rounded hover:bg-secondary/80 transition-colors">
          문화
        </button>
        <button class="bg-secondary text-secondary-foreground px-4 py-2 rounded hover:bg-secondary/80 transition-colors">
          여행
        </button>
      </div>
    </section>
  </main>
  <footer class="p-6 bg-muted text-muted-foreground">
    <p>© 2023 박민서의 블로그. All rights reserved.</p>
  </footer>
</div>


  </body>
</html>
