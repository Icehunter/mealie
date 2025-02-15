<template>
  <div class="mt-10">
    <v-row>
      <v-col cols="12" sm="12" md="4">
        <StatCard :icon="$globals.icons.primary">
          <template v-slot:after-heading>
            <div class="ml-auto text-right">
              <h2 class="body-3 grey--text font-weight-light">
                {{ $t("general.recipes") }}
              </h2>

              <h3 class="display-2 font-weight-light text--primary">
                <small> {{ statistics.totalRecipes }}</small>
              </h3>
            </div>
          </template>
          <template v-slot:actions>
            <div class="d-flex row py-3 justify-space-around">
              <v-btn small color="primary" :to="{ path: '/admin/toolbox/', query: { tab: 'organize', filter: 'tag' } }">
                <v-icon left> {{ $globals.icons.tags }} </v-icon>
                {{ $tc("tag.untagged-count", [statistics.untaggedRecipes]) }}
              </v-btn>
              <v-btn
                small
                color="primary"
                :to="{ path: '/admin/toolbox/', query: { tab: 'organize', filter: 'category' } }"
              >
                <v-icon left> {{ $globals.icons.tags }} </v-icon>
                {{ $tc("category.uncategorized-count", [statistics.uncategorizedRecipes]) }}
              </v-btn>
            </div>
          </template>
        </StatCard>
      </v-col>
      <v-col cols="12" sm="12" md="4">
        <StatCard :icon="$globals.icons.user">
          <template v-slot:after-heading>
            <div class="ml-auto text-right">
              <h2 class="body-3 grey--text font-weight-light">
                {{ $t("user.users") }}
              </h2>
              <h3 class="display-2 font-weight-light text--primary">
                <small> {{ statistics.totalUsers }}</small>
              </h3>
            </div>
          </template>
          <template v-slot:actions>
            <div class="ml-auto">
              <v-btn color="primary" small to="/admin/manage-users?tab=users">
                <v-icon left>{{ $globals.icons.user }}</v-icon>
                {{ $t("user.manage-users") }}
              </v-btn>
            </div>
          </template>
        </StatCard>
      </v-col>
      <v-col cols="12" sm="12" md="4">
        <StatCard :icon="$globals.icons.group">
          <template v-slot:after-heading>
            <div class="ml-auto text-right">
              <h2 class="body-3 grey--text font-weight-light">
                {{ $t("group.groups") }}
              </h2>

              <h3 class="display-2 font-weight-light text--primary">
                <small> {{ statistics.totalGroups }}</small>
              </h3>
            </div>
          </template>
          <template v-slot:actions>
            <div class="ml-auto">
              <v-btn color="primary" small to="/admin/manage-users?tab=groups">
                <v-icon left>{{ $globals.icons.group }}</v-icon>
                {{ $t("group.manage-groups") }}
              </v-btn>
            </div>
          </template>
        </StatCard>
      </v-col>
    </v-row>
    <v-row class="mt-10">
      <v-col cols="12" sm="12" lg="6">
        <EventViewer />
      </v-col>
      <v-col cols="12" sm="12" lg="6"> <BackupViewer /> </v-col>
    </v-row>
  </div>
</template>

<script>
import { api } from "@/api";
import StatCard from "@/components/UI/StatCard";
import EventViewer from "./EventViewer";
import BackupViewer from "./BackupViewer";
export default {
  components: { StatCard, EventViewer, BackupViewer },
  data() {
    return {
      statistics: {
        totalGroups: 0,
        totalRecipes: 0,
        totalUsers: 0,
        uncategorizedRecipes: 0,
        untaggedRecipes: 0,
      },
    };
  },
  mounted() {
    this.getStatistics();
  },
  methods: {
    async getStatistics() {
      this.statistics = await api.meta.getStatistics();
    },
  },
};
</script>

<style>
.grid-style {
  flex-grow: inherit;
  display: inline-flex;
  flex-wrap: wrap;
  gap: 10px;
}
</style>