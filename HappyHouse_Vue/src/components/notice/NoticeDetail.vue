!<template>
  <div class="container" align="center">
    <div class="col-lg-6" align="center">
      <h1>공지사항</h1>
      <div class="form-group" align="center">
        <h5>제목 :</h5>
        <textarea
          class="form-control"
          disabled="disabled"
          id="title"
          name="title"
          v-model="notice_title"
        ></textarea>
      </div>
      <div class="form-group2" align="center">
        <h5>내용 :</h5>
        <textarea
          class="form-control"
          disabled="disabled"
          rows="15"
          id="content"
          name="content"
          v-model="notice_content"
        ></textarea>
        <!-- <div>{{this.$session.get('noticeDetailKey')}}</div> -->
      </div>
    </div>
    <div v-if="this.$session.get('id') == 'admin'"> 
    <router-link to="/noticeModify" class="btn btn-primary">수정</router-link>
    <button type="button" class="btn btn-warning" @click="deleteHandler">삭제</button>
    <router-link to="/notice" class="btn btn-primary">취소</router-link>
    </div>
    <div v-if="this.$session.get('id') != 'admin'"> 
    <router-link to="/notice" class="btn btn-primary">취소</router-link>
    </div>
    
  </div>
</template>

<script>
import http from "@/util/http-common";
import { mapGetters } from "vuex";
export default {
  name: "read",
  data: function() {
    return {
      notice_no: this.$session.get("notice_no"),
      notice_id: this.$session.get("notice_id"),
      notice_title: this.$session.get("notice_title"),
      notice_content: this.$session.get("notice_content")
    };
  },
  computed: {
    ...mapGetters(["item"])
  },

  methods: {
    deleteHandler() {
      http
        .delete(`/notice/${this.$session.get("notice_no")}`)
        .then(({ data }) => {
          let msg = "삭제 처리시 문제가 발생했습니다.";
          if (data === "success") {
            msg = "삭제가 완료되었습니다.";
		  }
          alert(msg);
          this.$router.push('/notice');
        })
        .catch(() => {
          alert("삭제 처리시 에러가 발생했습니다.");
        });
	},
	created() {
    this.$store.dispatch(
      "getNotice",
      `/notice/${this.$session.get("noticeDetailKey")}`
    );
  },
  }
};
</script>

<style>
h1 {
  margin-bottom: 50px;
}
h3 {
  text-align: left;
}
button {
  margin-right: 60px;
}
h5{
  text-align: left;
}
</style>