<template>
    <div class="main-container">
        <aside class="aside__top">
            <!-- <span
                class="iconfont icon-nav toggleNavCollapse"
                :class="{ active: state.isSidebarNavCollapse }"
                @click="toggleNavCollapse"
            >
            </span> -->
            <el-breadcrumb separator="/">
                <transition-group name="breadcrumb">
                    <!-- 防止面包屑导航出现 首页/首页， v-if="route.name!='home'" -->
                    <el-breadcrumb-item
                        v-for="(route, i) in state.crumbList"
                        :key="route.name"
                        :to="{ name: route.name }"
                        :class="{
                            'is-last-link': i == state.crumbList.length - 1,
                        }"
                    >
                        <span v-if="route.name != 'home'">{{
                            route.meta.name
                        }}</span>
                    </el-breadcrumb-item>
                </transition-group>
            </el-breadcrumb>
            <div class="aside__top--right">
                <div class="email" @click="full">
                    <span class="el-icon-rank" style="font-size:18px"></span>
                </div>
                <div class="user-msg">
                    <!-- <img class="user-img" :src="require('@/assets/image/a.png')" alt=""> -->
                    <span class="user-name">{{ $t("name") }}</span>
                    <el-dropdown>
                        <i class="el-icon-arrow-down el-icon--right"></i>
                        <template #dropdown>
                            <el-dropdown-menu>
                                <el-dropdown-item>{{
                                    $t("navMy")
                                }}</el-dropdown-item>
                                <el-dropdown-item>{{
                                    $t("pwd")
                                }}</el-dropdown-item>
                            </el-dropdown-menu>
                        </template>
                    </el-dropdown>
                </div>
                <div class="quit-system" @click="loginOut">
                    <span class="iconfont icon-quit"></span>
                </div>
            </div>
        </aside>
        <!-- 内容容器 -->
        <router-view class="content"></router-view>
    </div>
</template>
<script lang="ts">
import { computed, defineComponent, reactive } from "vue";
import { useRouter } from "vue-router";
import { useStore } from "vuex";
import screenfull from "screenfull";
export default defineComponent({
    setup() {
        const store = useStore();
        const router = useRouter();
        let state: any = reactive({
            crumbList: computed(() => store.getters.getCrumbList),
            isSidebarNavCollapse: computed(
                () => store.getters.getIsSidebarNavCollapse
            ),
        });
        let toggleNavCollapse = () => {
            store.commit("toggleNavCollapse");
        };
        let full = () => {
           screenfull.isEnabled&&screenfull.toggle()
        };
        let loginOut = () => {
            window.localStorage.removeItem("token");

            router.push({ path: "/login" });
        };
        return {
            state,
            loginOut,
            full,
        };
    },
});
</script>
<style lang="scss" scoped>
.aside__top {
    border-bottom: 1px solid #e5e5e5;
    height: 50px;
    line-height: 50px;
    position: fixed;
    left: 200px;
    top: 0;
    right: 0;
    background: #fff;
    z-index: 1000;
    transition: left 0.25s;
    .toggleNavCollapse {
        display: inline-block;
        margin-left: 8px;
        padding: 0 10px;
        font-size: 26px;
        vertical-align: middle;
        color: #333;
        cursor: pointer;
        transition: all 0.5s;
        &.active {
            transform: rotate(90deg);
        }
    }

    .aside__top--right {
        position: absolute;
        right: 10px;
        top: -1px;
        bottom: 0px;
        > div {
            position: relative;
            display: inline-block;
            text-align: center;
            vertical-align: middle;
            margin-left: 10px;
            padding: 0 15px;
            cursor: pointer;
            &:hover::after {
                transform-origin: 0 0;
                transform: scaleX(1);
            }
            &:first-child:before {
                border: none;
            }
            &::after {
                content: "";
                position: absolute;
                left: 0;
                bottom: 0;
                width: 100%;
                height: 2px;
                background: #ef4747;
                transform: scaleX(0);
                transform-origin: right 0;
                transition: transform 0.5s;
            }
            &::before {
                content: "";
                position: absolute;
                height: 20px;
                top: 50%;
                left: -8px;
                margin-top: -10px;
                border-left: 1px solid #ccc;
            }
            &.email {
                i {
                    position: absolute;
                    left: 18px;
                    top: -12px;
                    border-radius: 20px;
                    background: red;
                    color: #fff;
                    text-align: center;
                    font-size: 12px;
                    line-height: 1.5;
                    min-width: 20px;
                    min-height: 20px;
                }
            }
            &.user-msg {
                .user-img {
                    width: 34px;
                    height: 34px;
                    border-radius: 50%;
                    vertical-align: middle;
                }
                .user-name {
                    color: #758eb5;
                    padding: 0 4px;
                }
            }
            .iconfont {
                position: relative;
                font-size: 24px;
                color: #758eb5;
            }
        }
    }
}
.breadcrumb-enter,
.breadcrumb-leave-active {
    opacity: 0;
    transform: translateX(20px);
}

.breadcrumb-enter-active,
.breadcrumb-leave-active {
    transition: all 0.6s;
}

.breadcrumb-leave-active {
    position: absolute;
}
</style>
