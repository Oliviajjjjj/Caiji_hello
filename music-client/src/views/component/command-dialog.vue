<template>
    <div>
        <el-dialog title="歌单推荐" v-model="dialogVisible" width="30%" :before-close="handleClose">
            <div class="command_block">

            </div>
            <template #footer>
                <el-button @click="dialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="jump">确 定</el-button>
            </template>
        </el-dialog>
    </div>
</template>
  
<script lang="ts">
import { defineComponent, nextTick, ref, toRefs, getCurrentInstance, computed, watch, reactive } from "vue";
import { useRouter } from "vue-router";
import mixin from "@/mixins/mixin";


export default defineComponent({
    components: {},
    props: {
        path: String,
    },
    setup(props) {
        const router = useRouter();
        const { path } = toRefs(props);
        const { proxy } = getCurrentInstance();
        const { routerManager } = mixin();
        const commandSongTopic = ref({
            value: ""
        })
        const dialogVisible = ref(false)
        const handleOpen = () => {
            dialogVisible.value = true
        }
        const handleClose = () => {
            dialogVisible.value = false
        }
        const jump = () => {
            console.log(commandSongTopic.value.value);
            
            // proxy.$store.commit("setSongDetails", commandSongTopic);
            routerManager(path.value, { path: `/${path.value}/${commandSongTopic.value.value}` });
        }
        
        return {
            jump,
            dialogVisible,
            commandSongTopic,
            handleOpen,
            handleClose
        };
    },
});
</script>
  
<style lang="scss" scoped>
.command_block {

}
</style>
  