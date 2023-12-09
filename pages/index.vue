<template>
  <div style="display: grid; padding: 2px; margin: 2px">
    <p class="my-2 pa-0 font-weight-bold">Steer Axles</p>
    <v-text-field
      outlined
      label="Only steer axles"
      v-model="at_steer_axles_value"
    ></v-text-field>
    <p
      v-if="steer_axles_information.message !== undefined"
      :class="`${steer_axles_information.color}--text font-weight-bold`"
    >
      {{ steer_axles_information.message }}
    </p>
    <p v-if="steer_axles_information.message !== undefined">
      Steer axle weight is {{ actual_weight_at_steer }}
    </p>
    <p class="pa-0 my-2 font-weight-bold">Drive Axles</p>
    <v-text-field
      label="total weight of (steer axles + drive axles)"
      outlined
      v-model="at_drive_axles_value"
    ></v-text-field>
    <p
      v-if="drive_axles_information.message !== undefined"
      :class="`${drive_axles_information.color}--text font-weight-bold`"
    >
      {{ drive_axles_information.message }}
    </p>
    <p v-if="drive_axles_information.message !== undefined">
      Drive axles weight is {{ actual_weight_at_drive }}
    </p>
    <p class="my-2 pa-0 font-weight-bold">Bridge Axles</p>
    <v-text-field
      label="total weight of (steer axle + drive axles + bridge axles)"
      outlined
      v-model="at_bridge_axles_value"
    ></v-text-field>
    <p
      v-if="bridge_axles_information.message !== undefined"
      :class="`${bridge_axles_information.color}--text font-weight-bold`"
    >
      {{ bridge_axles_information.message }}
    </p>
    <p v-if="bridge_axles_information.message !== undefined">
      Bridge axles weight is {{ actual_weight_at_bridge }}
    </p>
    <p class="my-2 pa-0 font-weight-bold">Pup Axles</p>
    <v-text-field
      label="total weight of (steer, drive, bridge and pup)"
      outlined
      v-model="at_pup_axles_value"
    ></v-text-field>
    <p
      v-if="pup_axles_information.message !== undefined"
      :class="`${pup_axles_information.color}--text font-weight-bold `"
    >
      {{ pup_axles_information.message }}
    </p>
    <p v-if="pup_axles_information.message !== undefined">
      Pup axles weight is {{ actual_weight_at_pup }}
    </p>
  </div>
</template>

<script>
export default {
  head: {
    title: 'Home',
  },
  data() {
    return {
      at_steer_axles_value: undefined,
      at_drive_axles_value: undefined,
      at_bridge_axles_value: undefined,
      at_pup_axles_value: undefined,

      actual_weight_at_steer: undefined,
      actual_weight_at_drive: undefined,
      actual_weight_at_bridge: undefined,
      actual_weight_at_pup: undefined,

      legal_steer_axles_limit: 5500,
      legal_drive_axles_limit: 17000,
      legal_bridge_axles_limit: 24000,
      legal_pup_axles_limit: 17000,

      steer_axles_information: {
        message: undefined,
        color: undefined,
      },
      drive_axles_information: {
        message: undefined,
        color: undefined,
      },
      bridge_axles_information: {
        message: undefined,
        color: undefined,
      },
      pup_axles_information: {
        message: undefined,
        color: undefined,
      },
    }
  },
  watch: {
    at_steer_axles_value(new_value, old_value) {
      this.at_steer_axles_value = parseInt(this.at_steer_axles_value)
      this.steer_axles_information.message = undefined
      this.steer_axles_information.color = undefined
      this.actual_weight_at_steer = undefined
      if (this.at_steer_axles_value > this.legal_steer_axles_limit) {
        this.steer_axles_information.message = `Steer axle over weight by ${
          this.at_steer_axles_value - this.legal_steer_axles_limit
        } Kgs`
        this.steer_axles_information.color = 'red'
        this.actual_weight_at_steer = this.at_steer_axles_value
      } else if (this.at_steer_axles_value < this.legal_steer_axles_limit) {
        this.steer_axles_information.message = `Steer axle under weight by ${
          5500 - this.at_steer_axles_value
        } Kgs`
        this.steer_axles_information.color = 'green'
        this.actual_weight_at_steer = this.at_steer_axles_value
      } else if (this.at_steer_axles_value === this.legal_steer_axles_limit) {
        this.steer_axles_information.message = `Steer axle weight ok`
        this.steer_axles_information.color = 'green'
        this.actual_weight_at_steer = this.legal_steer_axles_limit
      }
    },

    at_drive_axles_value(new_value, old_value) {
      this.at_drive_axles_value = parseInt(this.at_drive_axles_value)
      this.drive_axles_information.message = undefined
      this.drive_axles_information.color = undefined
      this.actual_weight_at_drive = undefined
      let expected_weight_limit =
        this.legal_drive_axles_limit + this.actual_weight_at_steer
      if (this.at_drive_axles_value > expected_weight_limit) {
        this.drive_axles_information.message = `Drive axles over weight by ${
          this.at_drive_axles_value - expected_weight_limit
        } Kgs`
        this.drive_axles_information.color = 'red'
        this.actual_weight_at_drive =
          this.at_drive_axles_value - this.at_steer_axles_value
      } else if (this.at_drive_axles_value < expected_weight_limit) {
        this.drive_axles_information.message = `Drive axles under weight by ${
          expected_weight_limit - this.at_drive_axles_value
        } Kgs`
        this.drive_axles_information.color = 'green'
        this.actual_weight_at_drive =
          this.at_drive_axles_value - this.at_steer_axles_value
      } else if ((this.at_drive_axles_value === expected_weight_limit)) {
        this.drive_axles_information.message = 'Drive axles weight ok'
        this.drive_axles_information.color = 'green'
        this.actual_weight_at_drive = this.legal_drive_axles_limit
      }
    },

    at_bridge_axles_value(new_value, old_value) {
      this.at_bridge_axles_value = parseInt(this.at_bridge_axles_value)
      this.bridge_axles_information.message = undefined
      this.bridge_axles_information.color = undefined
      let expected_weight_limit =
        this.legal_bridge_axles_limit +
        this.actual_weight_at_steer +
        this.actual_weight_at_drive
      if (this.at_bridge_axles_value > expected_weight_limit) {
        this.bridge_axles_information.message = `Bridge axles over weight by ${
          this.at_bridge_axles_value - expected_weight_limit
        } Kgs`
        this.bridge_axles_information.color = 'red'
        this.actual_weight_at_bridge =
          this.at_bridge_axles_value -
          this.actual_weight_at_drive -
          this.actual_weight_at_steer
      } else if (this.at_bridge_axles_value < expected_weight_limit) {
        this.bridge_axles_information.message = `Bridge axles under weight by ${
          expected_weight_limit - this.at_bridge_axles_value
        } Kgs`
        ;(this.bridge_axles_information.color = 'green'),
          (this.actual_weight_at_bridge =
            this.at_bridge_axles_value -
            this.actual_weight_at_drive -
            this.actual_weight_at_steer)
      } else if ((this.at_bridge_axles_value === expected_weight_limit)) {
        ;(this.bridge_axles_information.message = 'Bridge axles weight ok'),
          (this.bridge_axles_information.color = 'green')
        this.actual_weight_at_bridge = this.legal_bridge_axles_limit
      }
    },

    at_pup_axles_value(new_value, old_value) {
      this.at_pup_axles_value = parseInt(this.at_pup_axles_value)
      ;(this.pup_axles_information.message = undefined),
        (this.pup_axles_information.color = undefined)
      let expected_weight_limit =
        this.legal_pup_axles_limit +
        this.actual_weight_at_steer +
        this.actual_weight_at_drive +
        this.actual_weight_at_bridge
      if (this.at_pup_axles_value > expected_weight_limit) {
        this.pup_axles_information.message = `Pup over ${
          this.at_pup_axles_value - expected_weight_limit
        } Kgs`
        this.pup_axles_information.color = 'red'
        this.actual_weight_at_pup =
          this.at_pup_axles_value -
          this.actual_weight_at_bridge -
          this.actual_weight_at_drive -
          this.actual_weight_at_steer
      } else if (this.at_pup_axles_value < expected_weight_limit) {
        this.pup_axles_information.message = `Pup under by ${
          expected_weight_limit - this.at_pup_axles_value
        } Kgs`
        this.pup_axles_information.color = 'green'
        this.actual_weight_at_pup =
          this.at_pup_axles_value -
          this.actual_weight_at_bridge -
          this.actual_weight_at_drive -
          this.actual_weight_at_steer
      } else if (this.at_pup_axles_value === expected_weight_limit) {
        this.pup_axles_information.message = 'Pup axle weight ok'
        this.pup_axles_information.color = 'green'
        this.actual_weight_at_pup = this.legal_pup_axles_limit
      }
    },
  },
}
</script>

<style lang="scss" scoped>
* {
  padding: 0px;
  margin: 0px;
}
</style>
