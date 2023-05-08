<template>
  <!--轮播图-->
  <template v-if="swiperList.length > 0">
    <el-carousel class="swiper-container" type="card" height="20vw" :interval="4000">
      <el-carousel-item v-for="(item, index) in swiperList" :key="index">
        <img :src="HttpManager.attachImageUrl(item.pic)" />
      </el-carousel-item>
    </el-carousel>
  </template>
  <!--热门歌单-->
  <play-list class="play-list-container" title="歌单" path="song-sheet-detail" :playList="songList"></play-list>
  <!--热门歌手-->
  <play-list class="play-list-container" title="歌手" path="singer-detail" :playList="singerList"></play-list>
  <!-- 推荐歌单 -->
  <command-dialog ref="commandDialogRef" path="song-sheet-detail"></command-dialog>
  <!-- <el-dialog title="提示" v-model="dialogVisible" width="30%" :before-close="handleClose">
      <span>这是一段信息</span>
      <template #footer>
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </template>
  </el-dialog> -->
</template>

<script lang="ts" setup>
import { ref, onMounted } from "vue";
import PlayList from "@/components/PlayList.vue";
import {  NavName } from "@/enums";
import { HttpManager } from "@/api";
import mixin from "@/mixins/mixin";
import { log } from "console";
import commandDialog from "@/views/component/command-dialog.vue"

const songList = ref([]); // 歌单列表
const singerList = ref([]); // 歌手列表
const swiperList = ref([]);// 轮播图 每次都在进行查询
const { changeIndex } = mixin();
const commandDialogRef = ref()
const commandSongTopic = ref({})

try {
  
  HttpManager.getBannerList().then((res) => {
    swiperList.value = (res as ResponseBody).data.sort();
  });

  HttpManager.getSongList().then((res) => {
    songList.value = (res as ResponseBody).data.sort().slice(0, 10);
    commandSongTopic.value = songList.value[0]
    commandDialogRef.value.commandSongTopic.value = commandSongTopic.value.id
    
  });

  HttpManager.getAllSinger().then((res) => {
    singerList.value = (res as ResponseBody).data.sort().slice(0, 10);
  });

  onMounted(() => {
    changeIndex(NavName.Home);
    commandDialogRef.value.handleOpen()
    
    // console.log(commandSongTopic.value);
    
    
  });
} catch (error) {
  console.error(error);
}
</script>

<style lang="scss" scoped>
@import "@/assets/css/var.scss";

/*轮播图*/
.swiper-container {
  width: 90%;
  margin: auto;
  padding-top: 20px;
  img {
    width: 100%;
  }
}

.swiper-container:deep(.el-carousel__indicators.el-carousel__indicators--outside) {
  display: inline-block;
  transform: translateX(30vw);
}

.el-slider__runway {
  background-color: $color-blue;
}
</style>
