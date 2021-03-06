<template>
  <v-card hover class="tenant">
    <v-card-text :style="tenantPanelStyle(tenant)">
      <div class="tenant-logo" :style="tenantLogoStyle(tenant)">
      </div>
      <div class="tenant-divider"></div>
      <div class="tenant-name headline ellipsis">
        {{tenant.name}} ({{tenant.id}})
      </div>
      <div class="tenant-actions">
        <v-btn @click.native.stop="onConfigureTenant"
          class="blue white--text tenant-configure ma-0" >
          <v-icon left dark fa>cogs</v-icon>
          Configure
        </v-btn>
        <v-btn @click.native.stop="onOpenTenant"
          class="green white--text tenant-open" >
          <v-icon left dark fa>database</v-icon>
          Manage Data
        </v-btn>
      </div>
      <div v-if="tenantStarted">
        <v-icon class="tenant-symbol green--text fa-lg" fa>check-circle</v-icon>
        <span class="tenant-message">Tenant is Running</span>
      </div>
      <div v-if="tenantStopped">
        <v-icon class="tenant-symbol grey--text fa-lg" fa>power-off</v-icon>
        <span class="tenant-message">Tenant is Stopped</span>
      </div>
      <div v-if="tenantError">
        <v-icon class="tenant-symbol red--text fa-lg" fa>warning</v-icon>
        <span class="tenant-message">Tenant Failed to Start</span>
      </div>
    </v-card-text>
  </v-card>
</template>

<script>
export default {

  data: () => ({
  }),

  props: ['tenant'],

  computed: {
    tenantStarted: function () {
      return this.tenant.engineState &&
        this.tenant.engineState.lifecycleStatus === 'Started'
    },
    tenantStopped: function () {
      return this.tenant.engineState &&
        this.tenant.engineState.lifecycleStatus === 'Stopped'
    },
    tenantError: function () {
      return this.tenant.engineState &&
        this.tenant.engineState.lifecycleStatus === 'LifecycleError'
    },
    tenantHeaderColor: function () {
      if (this.tenantStarted) {
        return '#060'
      } else if (this.tenantStopped) {
        return '#ddd'
      } else if (this.tenantError) {
        return '#d00'
      }
    }
  },

  methods: {
    // Styling for tenant panel.
    tenantPanelStyle: function (tenant) {
      return {
        'border-top': '5px solid ' + this.tenantHeaderColor
      }
    },

    // Styling for tenant logo.
    tenantLogoStyle: function (tenant) {
      return {
        'background': 'url(' + tenant.logoUrl + ')',
        'background-size': 'contain',
        'background-repeat': 'no-repeat',
        'background-position': '50% 50%'
      }
    },

    // Open tenant.
    onOpenTenant: function () {
      this.$emit('openTenant', this.tenant)
    },

    // Configure tenant.
    onConfigureTenant: function () {
      this.$emit('configureTenant', this.tenant)
    }
  }
}
</script>

<style scoped>
.tenant {
  min-height: 90px;
}
.tenant-symbol {
  position: absolute;
  top: 55px;
  left: 270px;
}
.tenant-message {
  position: absolute;
  top: 53px;
  left: 294px;
  font-size: 14px;
}
.tenant-logo {
  position: absolute;
  top: 10px;
  left: 30px;
  bottom: 10px;
  width: 200px;
}
.tenant-divider {
  position: absolute;
  top: 10px;
  left: 230px;
  bottom: 10px;
  width: 20px;
  border-right: 1px solid #eee;
}
.tenant-name {
  position: absolute;
  top: 20px;
  left: 270px;
  right: 10px;
}
.tenant-actions {
  position: absolute;
  right: 10px;
  top: 22px;
}
.ellipsis {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
