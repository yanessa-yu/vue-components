
<template>
    <p class="common-loading"><i></i>{{loadingText}}</p>
</template>

<script>
    export default {
        name: 'CommonItem',
        props: {
            loadingText: {
                type: String,
                default: '正在加载中'
            }
        }
    }
</script>

<style>
    .common-loading{
        line-height: 0.4rem;
        font-size: 0.28rem;
        color: #999;
        text-align: center;
    }
    .common-loading i{
        position: relative;
        display: inline-block;
        width: 0.25rem;
        height: 0.25rem;
        top: 0.02rem;
        margin: 0 0.1rem 0 0;
        background:url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAXCAMAAAAm/38fAAAAZlBMVEUAAACZmZmZmZmdnZ2ampqampqbm5uampqampqbm5ucnJykpKSampqampqbm5uampqenp6ampqampqampqZmZmZmZmampqampqampqbm5ubm5uenp6ampqampqbm5uZmZmampqZmZkv8ZZDAAAAIXRSTlMA5uwVXFY2pXE9EArVh01CH9urSfrLvLWXfWcbzI1PLSulnLo0AAAAu0lEQVQoz3WRVxLDIAxE14XmXuOWxv0vGSkO40bejwRvYBYBRua5pBLVAge0JTSQjhJ7qo5FV+FMbL/Ebv0IfgeLVRROhHZamyTg/SBxosxda8gEBh7SskxxoVWqhQdhCXG6QWUCPYv+8oCQwlkuB4y1GRoWzfn+vEKUkY7gQ0r84a0GAQ9RuJ8UlqHWu+kqODiNAdOyGIFUTwuA+/YFN5phtZYEmKm4lM+YusR9iplP6TnDCx7KOtTb6gP0RQ9esNjisQAAAABJRU5ErkJggg==') no-repeat center center / 0.25rem 0.25rem;
        animation: loading 0.8s infinite linear;
    }
    @keyframes loading{
        from {transform: rotate(0);}
        to {transform: rotate(360deg);}
    }

</style>

