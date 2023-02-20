<template>
    <div class="container">
        <div class="main">
            <div class="home-desc">
                <h1 class="home-tit">TODO-List로<br>일과 생활을 관리하세요!</h1>
                <p class="home-cont">간단한 정리로 시작하는 편리한 일상</p>
                <button class="btn home-btn"><router-link :to="{name:'Todos'}">지금 시작하기</router-link></button>
            </div>
            <div class="home-image">
                <div></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    
}
</script>

<style>
    .main{display: flex; justify-content: space-between; align-items: center; margin-top: 50px;}
    .home-desc{flex-basis: 50%;}
    .home-tit{font-size: 2.3em;}
    .home-cont{font-size: 23px; margin-bottom: 15px; color: #575757;}
    .home-btn{font-size: 16px; border-radius: 5px;}
    .home-image{display: inline-block; flex-basis: 50%;  padding: 20px 0 20px; width: 400px; height: 600px;}
    .home-image div{width: 100%; height: 100%; background: url(./image/plan.jpg) no-repeat center center/100%; border-radius: 20px; box-shadow: 0 5px 10px #8b8b8b;}
    .home-btn a{text-decoration: none; color: #fff;}
</style>