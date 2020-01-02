<template>
  <div>
    <div class="display-4 " align="center" style="margin-top: 40px; color: #41b883">
      Clube Desportivo
    </div>

    <div class="headline" align="center" style="color: dimgrey; margin-top: 20px; margin-bottom: 20px;">
      Desafia-te!
    </div>

    <v-carousel
    cycle
    style="height: 400px">
      <v-carousel-item
        v-for="(item,i) in items"
        :key="i"
        :src="item.src"

        reverse-transition="fade-transition"
        transition="fade-transition"
      ></v-carousel-item>
    </v-carousel>
  </div>

</template>

<script>
export default {
  data () {
    return {
      items: [
        {
          src: 'https://images.pexels.com/photos/863988/pexels-photo-863988.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940',
        },
        {
          src: 'https://images.pexels.com/photos/618612/pexels-photo-618612.jpeg?auto=compress&cs=tinysrgb&dpr=3&h=750&w=1260',
        },
        {
          src: 'https://images.pexels.com/photos/248547/pexels-photo-248547.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940',
        },
        {
          src: 'https://images.pexels.com/photos/949126/pexels-photo-949126.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940',
        },
      ],
    }
  },
  created () {
  }
}
</script>
<style>
  .group-form{
    margin-top: 8px;
    margin-bottom: 13px;
    margin-left: 50px;
    font-size: 16px;
  }
  .input{
    font-weight: bold;
    margin-bottom: 3px;
  }
</style>
