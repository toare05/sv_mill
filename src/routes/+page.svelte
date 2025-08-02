<script>
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
    import { Button } from '$lib/components/ui/button';
    import { Card, CardContent, CardDescription, CardFooter, CardHeader, CardTitle } from '$lib/components/ui/card';
  
    // 블로그 포스트 타입 정의
    /** @typedef {{ slug: string; title: string; date: string; description: string }} BlogPost */
  
    /** @type {BlogPost[]} */
    let recentPosts = [];
  
    // 컴포넌트 마운트 시 최신 블로그 포스트 가져오기
    onMount(async () => {
      try {
        const response = await fetch('/api/recent-posts');
        if (response.ok) {
          recentPosts = await response.json();
        }
      } catch (error) {
        console.error('블로그 포스트를 가져오는 중 오류 발생:', error);
      }
    });
  
    // 네비게이션 핸들러
    function handleScoreCalculator() {
      goto('/scorecalculator');
    }

    function handleMoneyCalculator() {
      goto('/moneycalculator');
    }
  </script>
  
  <main class="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100 py-8 px-4 md:py-12 md:px-8">
    <div class="max-w-7xl mx-auto">
      <div class="text-center mb-12">
        <h1 class="text-4xl font-bold mb-3">계산기밀</h1>
        <p class="text-gray-600 text-lg max-w-2xl mx-auto">
          대한민국 군 관련 다양한 계산기를 제공합니다. 
          공군 지원 점수와 장병내일준비적금 수령액 계산을 도와드립니다.
        </p>
      </div>
  
      <div class="grid grid-cols-1 md:grid-cols-2 gap-8 max-w-5xl mx-auto">
        <!-- 공군 점수 계산기 카드 -->
        <Card class="bg-white rounded-xl shadow-md transition-all hover:shadow-lg border border-gray-100">
          <CardHeader class="pb-2">
            <CardTitle class="text-2xl font-bold text-blue-700">공군 지원 점수 계산기</CardTitle>
            <CardDescription>공군 지원을 위한 1차 점수를 계산해보세요</CardDescription>
          </CardHeader>
          <CardContent class="pb-4">
            <p class="text-gray-600">
              자격/면허, 전공, 출결, 가산점 등을 입력하여 공군 지원 1차 점수를 실시간으로 계산하고, 
              선발 점수와 비교해볼 수 있습니다.
            </p>
          </CardContent>
          <CardFooter>
            <Button 
              class="w-full bg-blue-600 hover:bg-blue-700" 
              onclick={handleScoreCalculator}
            >
              점수 계산하기
            </Button>
          </CardFooter>
        </Card>
  
        <!-- 장병내일준비적금 계산기 카드 -->
        <Card class="bg-white rounded-xl shadow-md transition-all hover:shadow-lg border border-gray-100">
          <CardHeader class="pb-2">
            <CardTitle class="text-2xl font-bold text-green-700">군적금(장병내일준비적금) 계산기</CardTitle>
            <CardDescription>군 적금 만기시 예상 수령액을 확인해보세요</CardDescription>
          </CardHeader>
          <CardContent class="pb-4">
            <p class="text-gray-600">
              매월 적립액, 이자율, 복무기간 등을 입력하여 군적금(장병내일준비적금) 만기시 
              받게 될 최종 수령액을 계산할 수 있습니다.
            </p>
          </CardContent>
          <CardFooter>
            <Button 
              class="w-full bg-green-600 hover:bg-green-700" 
              onclick={handleMoneyCalculator}
            >
              수령액 계산하기
            </Button>
          </CardFooter>
        </Card>
      </div>
  
      <!-- 최신 블로그 포스트 섹션 -->
      <div class="mt-16 max-w-5xl mx-auto">
        <div class="text-center mb-10">
          <h2 class="text-3xl font-bold mb-2">최신 블로그 포스트</h2>
          <p class="text-gray-600">군 생활과 관련된 유용한 정보를 확인하세요</p>
        </div>
  
        {#if recentPosts.length > 0}
          <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            {#each recentPosts as post (post.slug)}
              <Card class="bg-white rounded-lg shadow-sm hover:shadow-md transition-all">
                <CardHeader class="pb-2">
                  <CardTitle class="text-xl font-bold text-blue-700 line-clamp-2">{post.title}</CardTitle>
                  <CardDescription>
                    {new Date(post.date).toLocaleDateString('ko-KR')}
                  </CardDescription>
                </CardHeader>
                <CardContent class="pb-4">
                  <p class="text-gray-600 line-clamp-3">{post.description}</p>
                </CardContent>
                <CardFooter>
                  <Button 
                    variant="outline" 
                    class="w-full" 
                    on:click={() => navigateTo(`/blog/${post.slug}`)}
                  >
                    자세히 보기
                  </Button>
                </CardFooter>
              </Card>
            {/each}
          </div>
        {:else}
          <div class="text-center">
            <p class="text-gray-500 mb-4">아직 게시된 블로그 글이 없습니다.</p>
            <Button on:click={() => navigateTo('/blog')}>블로그 방문하기</Button>
          </div>
        {/if}
  
        <div class="text-center mt-8">
          <Button 
            variant="outline" 
            class="px-6" 
            on:click={() => navigateTo('/blog')}
          >
            모든 블로그 글 보기
          </Button>
        </div>
      </div>
    </div>
  </main>