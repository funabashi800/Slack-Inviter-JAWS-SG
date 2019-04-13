<template>
  <section class="container">
    <div>
      <Icon />
      <Input :info=info />
    </div>
  </section>
</template>

<script>
import Icon from '~/components/Icon.vue'
import Input from '~/components/Input.vue'
import axios from 'axios'

export default {
  async asyncData(){
    let {data} = await axios.get('https://slack.com/api/users.list', {
      params: {
        token: process.env.SLACK_TOKEN,
        presence: true
      }
    })
    let total = data.members.length;
    let active = data.members.filter(user => {
      return user.presence === 'active'
    }).length
    return {
        info: {
          total: total,
          active: active
        }
    }
},
  components: {
    Icon,
    Input
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}


</style>
