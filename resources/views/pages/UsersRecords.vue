<template>
  <section class="user">
    <div class="top-list">
      <h1>Личный кабинет</h1>
      <a href="/logout" class="logout">Выход</a>
    </div>
    <div class="menu-user">
      <router-link to="/profile" class="personal">Личные данные</router-link>
      <router-link to="/userrecords" class="records">Записи на прием </router-link>
    </div>
    <div class="cards-record">
      <div class="card" v-for="record in activeRecordsInfo">
        <div class="top">
          <div class="date-time">
            <p class="date">{{record['date'].split('-')[2] + '.' + record['date'].split('-')[1]}}</p>
            <p class="time">{{record['time']}}</p>
          </div>
          <p class="status-on">Активна</p>
        </div>
        <div class="info">
          <div class="title">Врач</div>
          <div class="info-doctor">
            <p class="unit">{{ record['doctor_type_name'] }}</p>
            <p class="name">{{ record['doctor_name'] }}</p>
          </div>
        </div>
        <a :href="`/reject/${record['id']}`">
          <button>Отменить запись</button>
        </a>
      </div>
      <div class="card" v-for="record in lostRecordsInfo">
        <div class="top">
          <div class="date-time">
            <p class="date">{{record['date'].split('-')[2] + '.' + record['date'].split('-')[1]}}</p>
            <p class="time">{{record['time']}}</p>
          </div>
          <p class="status-off">Прошла</p>
        </div>
        <div class="info">
          <div class="title">Врач</div>
          <div class="info-doctor">
            <p class="unit">{{ record['doctor_type_name'] }}</p>
            <p class="name">{{ record['doctor_name'] }}</p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import {defineComponent} from "vue";
import axios from "axios";

export default defineComponent(
    {
      data: () => {
        return {
          informationUser: {},
          lostRecordsInfo: [],
          activeRecordsInfo: []
        }
      },
      mounted() {
          axios.get('/user').then(response => {
            this.informationUser = response.data

            axios.get(`/getLostUserRecords/${this.informationUser['id']}`).then(resp => {
              this.lostRecordsInfo = resp.data

              console.log(this.lostRecordsInfo)
            })
            axios.get(`/getActiveUserRecords/${this.informationUser['id']}`).then(resp => {
              this.activeRecordsInfo = resp.data
            })
          })

      }
    }
)
</script>

<style scoped>
.user{
  width: 1440px;
  margin: 72px auto 0 auto;
}
.user h1{
  font-size: 28px;
  color: #0F2B56;
}
.user .menu-user{
  display: flex;
  gap: 44px;
}

.user .menu-user .personal{
  text-decoration: none;
  font-size: 18px;
  color: #7A8FAD;
}
.user .menu-user .records{
  text-decoration: none;
  color: #719FE7;
  font-size: 18px;
}

.user .cards-record{
  display: flex;
  gap: 37px;
  margin-top: 51px;
  flex-wrap: wrap;
}
.user .cards-record .card{
  padding: 36px 28px;
  background-color: white;
  border-radius: 12px;
  width: 500px;
  height: 200px;
}
.user .cards-record .card .top{
  display: flex;
  justify-content: space-between;
}
.user .cards-record .card .top p{
  margin-top: 0;
}
.user .cards-record .card .top .date-time{
  display: flex;
  gap: 10px;
  font-size: 24px;
  color: #0F2B56;
}
.user .cards-record .card .top .status-on{
  color: #36DF27;
  font-size: 20px;
  font-weight: 500;
}
.user .cards-record .card .top .status-off{
  font-size: 20px;
  color: #BAC9DF;
}
.user .cards-record .card .info{
  display: flex;
  flex-direction: column;
  gap: 14px;
  color: #0F2B56;
  font-size: 20px;
}
.user .cards-record .card .info p{
margin: 0;
}
.user .cards-record .card .info .title{
  font-weight: 500;
}
.user .cards-record .card .info .info-doctor{
  display: flex;
  gap: 30px;
}
.user .cards-record .card .info .info-doctor .unit{
  font-weight: 300;
}
.user .cards-record .card .info .info-doctor .name{
  font-weight: 500;
}
.user .cards-record .card button{
  margin-top: 51px;
  padding: 13px 18px;
  font-size: 16px;
  color: white;
  font-weight: bold;
  background-color: #80B4FF;
  border-radius: 64px;
  border: none;
  box-shadow: 0 1px 0 1px rgb(33, 106, 210);

}

.top-list{
  display: flex;
  gap: 41px;
  align-items: center;
}

.top-list .logout{
  text-decoration: none;
  font-family: 'Inter', sans-serif;
  font-size: 22px;
  font-weight: 500;
  color: #0F2B56;
}
</style>