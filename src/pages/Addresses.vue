<template>
  <div>
    <div class="row">
      <card>
        <div class="card-header mb-5">
          <h5 class="card-category">Hakken</h5>
          <h3 class="card-title">Host &amp; IP Address Results</h3>
        </div>
        <div class="card-body">
          <p>Review all identified assets, how they were discovered and when..</p>
        </div>
      </card>
    </div>
    <div v-if="isLoading" class="col-md-6">
      <h4>Loading...</h4>
    </div>
    <div v-else-if="!hasGroups" class="col-md-6">
      <p>
        You have no groups,
        <router-link to="/groups/new">create one first</router-link>.
      </p>
    </div>
    <div v-else class="row" v-for="(value, key) in groups" :key="key">
      <card>
        <div class="card-header mb-5">
          <h3 class="card-title">{{ value.group_name }} group - Asset List</h3>
        </div>
        <div class="card-body">
          <div class="row">
            <div v-if="canPortScan && groupPortScanEnabled(value)" class="col-sm">
              <router-link :to="'/portstable/'+ value.group_id">
                <stats-card
                  title="Port Scan Results"
                  type="info"
                  class="results-card"
                  icon="tim-icons icon-molecule-40"
                >
                  <div slot="footer">Port Scan Results of all assets in scope</div>
                </stats-card>
              </router-link>
            </div>
            <div class="col-sm">
              <router-link :to="'/hoststable/'+ value.group_id">
                <stats-card
                  title="Hostnames"
                  type="info"
                  class="results-card"
                  icon="tim-icons icon-laptop"
                >
                  <div slot="footer">All hostnames identified with a list of IP addresses</div>
                </stats-card>
              </router-link>
            </div>
            <div class="col-sm">
              <router-link :to="'/addresstable/'+ value.group_id">
                <stats-card
                  title="All Addresses"
                  type="info"
                  class="results-card"
                  icon="tim-icons icon-vector"
                >
                  <div slot="footer">All assets discovered, nameserver records, timestamps and more</div>
                </stats-card>
              </router-link>
            </div>
          </div>
        </div>
      </card>
    </div>
  </div>
</template>
<script>
import { mapGetters } from 'vuex';
import { Tabs, TabPane } from 'src/components';
import StatsCard from 'src/components/Cards/StatsCard';

export default {
  components: {
    Tabs,
    TabPane,
    StatsCard
  },
  data() {
    return {};
  },
  computed: {
    ...mapGetters('scangroup', ['groups', 'isLoading']),
    ...mapGetters('user', ['canPortScan']),

    hasGroups() {
      return Object.entries(this.groups).length !== 0;
    }
  },
  methods: {
    groupPortScanEnabled(group) {
      if (
        group.module_configurations == undefined ||
        group.module_configurations.port_module === undefined ||
        group.module_configurations.port_module.port_scan_enabled === undefined
      ) {
        return false;
      }
      return group.module_configurations.port_module.port_scan_enabled === true;
    }
  },
  created() {},
  mounted() {}
};
</script>
<style lang="scss">
.results-card {
  background-color: lighten(#222a42, 4%);
}
</style>
