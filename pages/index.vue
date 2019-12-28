<template>

    <div >
      <ola/>
      {{this.true}}
    </div>

</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'
import ola from '~/components/ola.vue'

export default {
  data: function () {
    return{
      true: '2'

    }

  },
  components: {
    Logo,
    VuetifyLogo,
    ola
  }
}
</script>
