<template>
  <div class="user-profile">
    <!-- 导航栏 -->
    <van-nav-bar
      title="个人信息"
      class="page-nav-bar"
      left-arrow
      @click-left="$router.back()"
    />

    <!-- 个人信息 -->
    <van-cell-group>
      <van-cell title="头像" is-link>
        <van-image class="avatar" round fit="cover" :src="user.photo" />
      </van-cell>
      <van-cell
        title="昵称"
        :value="user.name"
        is-link
        @click="isUpdateNameShow = true"
      />
      <van-cell
        title="性别"
        :value="user.gender === 0 ? '男' : '女'"
        is-link
        @click="isUpdateGenderShow = true"
      />
      <van-cell
        title="生日"
        :value="user.birthday"
        is-link
        @click="isUpdateBirthdayShow = true"
      />
    </van-cell-group>

    <!-- 编辑昵称 -->
    <van-popup v-model="isUpdateNameShow" position="bottom" style="height: 100%"
      ><UpdateName
        v-if="isUpdateNameShow"
        @close="isUpdateNameShow = false"
        v-model="user.name"
      ></UpdateName>
    </van-popup>

    <!-- 编辑性别 -->
    <van-popup v-model="isUpdateGenderShow" position="bottom">
      <UpdateGender
        @close="isUpdateGenderShow = false"
        v-model="user.gender"
      ></UpdateGender>
    </van-popup>

    <!-- 编辑生日 -->
    <van-popup v-model="isUpdateBirthdayShow" position="bottom">
      <UpdateBirthday
        v-if="isUpdateBirthdayShow"
        v-model="user.birthday"
        @close="isUpdateBirthdayShow = false"
      ></UpdateBirthday>
    </van-popup>
  </div>
</template>

<script>
import { getUserProfile } from "@/api/user";
import UpdateName from "./components/update-name";
import UpdateGender from "./components/update-gender";
import UpdateBirthday from "./components/update-birthday";
// import UpdatePhoto from './components/update-photo'

export default {
  name: "UserProfile",
  components: {
    UpdateName,
    UpdateGender,
    UpdateBirthday,
    // UpdatePhoto
  },
  props: {},
  data() {
    return {
      user: {}, // 个人信息
      isUpdateNameShow: false,
      isUpdateGenderShow: false,
      isUpdateBirthdayShow: false,
      // isUpdatePhotoShow: false,
      // img: null // 预览的图片
    };
  },
  computed: {},
  watch: {},
  created() {
    this.loadUserProfile();
  },
  mounted() {},
  methods: {
    async loadUserProfile() {
      try {
        const { data } = await getUserProfile();
        this.user = data.data;
        console.log(data);
      } catch (err) {
        this.$toast("数据获取失败");
      }
    },
    // onFileChange () {
    //   // 获取文件对象
    //   const file = this.$refs.file.files[0]
    //   // 基于文章对象获取 blob 数据
    //   this.img = window.URL.createObjectURL(file)
    //   // 展示预览图片弹出层
    //   this.isUpdatePhotoShow = true
    //   // file-input 如果选了同一个文件不会触发 change 事件
    //   // 解决办法就是每次使用完毕，把它的 value 清空
    //   this.$refs.file.value = ''
    // }
  },
};
</script>

<style scoped lang="less">
.user-profile {
  .avatar {
    width: 60px;
    height: 60px;
  }

  .van-popup {
    background-color: #f5f7f9;
  }

  .photo-cell {
    .van-cell__value {
      display: flex;
      flex-direction: row-reverse;
    }
  }
}
</style>
