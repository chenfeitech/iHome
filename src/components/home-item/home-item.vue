<template>
    <view class="home-item">
        <view class="home-item-title">
            {{title}}
        </view>
        <scroll-view
            class="home-item-list"
            :class="{'is-column': isColumn}"
            :scroll-x="true"
            :show-scrollbar="false"
            :enable-flex="true">
            <view
                class="post-container"
                :class="{'post-container-column': isColumn}"
                v-for="(item, index) in list"
                :key="index"
                @click="goToDetail(item)">
                <image
                    class="post-image"
                    :src="item.postImageUrl"
                    :alt="item.title">
                </image>
            </view>
        </scroll-view>
    </view>
</template>

<script>
export default {
    name: "home-item",
    props: {
        title: {
            type: String,
            required: true
        },
        isColumn: {
            type: Boolean,
            default: true
        },
        list: {
            type: Array,
            default() {
                return []
            }
        }
    },
    methods: {
        goToDetail(item) {
            uni.navigateTo({
                url: '/pages/furniture-detail/index?id=' + item.id
            })
        }
    }
}
</script>

<style lang="scss" scope>
    .home-item {
        width: 100%;
        margin-top: 36rpx;
        box-sizing: border-box;
        &-title {
            padding-left: 40rpx;
            font-size: 28rpx;
            font-weight: bold;
            line-height: 42rpx;
            margin-bottom: 20rpx;
        }
        .home-item-list {
            width: 100%;
            height: 240rpx;
            white-space: nowrap;
            &.is-column {
                height: 420rpx;
            }
            .post-container {
                width: 420rpx;
                height: 100%;
                margin-right: 36rpx;
                display: inline-block;
                border-radius: 40rpx;
                overflow: hidden;
                &.post-container-column {
                    width: 280rpx;
                }
                &:nth-of-type(1) {
                    margin-left: 40rpx;
                }
                &:last-of-type {
                    margin-right: 40rpx;
                }
            }
            .post-image {
                width: 100%;
                height: 100%;
            }
        }
    }
</style>