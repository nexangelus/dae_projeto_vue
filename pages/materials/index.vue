<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Materials</h2>
    </div>
    <p>
      <nuxt-link class="btn btn-success" :to="{name: 'materials-create'}" title="Create Material Manually">
        <fa :icon="['fas', 'plus']"/>
      </nuxt-link>
      <nuxt-link class="btn btn-success" :to="{name: 'materials-upload'}" title="Import Materials from Excel">
        <fa :icon="['fas', 'upload']"/>
      </nuxt-link>
    </p>

    <div class="accordion" role="tablist">
      <b-card no-body class="mb-1">
        <b-card-header header-tag="header" class="p-1" role="tab">
          <b-button block v-b-toggle.accordion-profile variant="info">Profiles</b-button>
        </b-card-header>
        <b-collapse id="accordion-profile" visible accordion="accordion-materials">
          <b-card-body>
            <b-form-input
              id="filter-input-profiles"
              v-model="tables.profiles.filter"
              type="search"
              placeholder="Type to Search"
            ></b-form-input>
            <b-table id="table-profiles" striped hover :items="profiles" :fields="profilesFields"
                     :per-page="tables.perPage" :current-page="tables.profiles.currentPage"
                     :filter="tables.profiles.filter" @filtered="onFilteredProfiles">
              <template #cell(actions)="data">
                <nuxt-link class="btn btn-primary" :to="`/materials/${data.item.id}`">
                  <fa :icon="['fa', 'info-circle']"/>
                </nuxt-link>
                <a class="btn btn-danger" v-if="data.item.manufacturerUsername == manufacturerUsername"
                   @click="remove(data.item.id)">
                  <fa :icon="['fas', 'trash']"/>
                </a>
              </template>
            </b-table>
            <b-pagination
              v-model="tables.profiles.currentPage"
              :total-rows="tables.profiles.rows"
              :per-page="tables.perPage"
              limit="10"
              aria-controls="table-profiles"
            ></b-pagination>
          </b-card-body>
        </b-collapse>
      </b-card>

      <b-card no-body class="mb-1">
        <b-card-header header-tag="header" class="p-1" role="tab">
          <b-button block v-b-toggle.accordion-sheet variant="info">Sheets</b-button>
        </b-card-header>
        <b-collapse id="accordion-sheet" accordion="accordion-materials">
          <b-card-body>
            <b-form-input
              id="filter-input-sheets"
              v-model="tables.sheets.filter"
              type="search"
              placeholder="Type to Search"
            ></b-form-input>
            <b-table id="table-sheets" striped hover :items="sheets" :fields="sheetsFields" :per-page="tables.perPage"
                     :current-page="tables.sheets.currentPage">
              <template #cell(actions)="data">
                <nuxt-link class="btn btn-primary" :to="`/materials/${data.item.id}`">
                  <fa :icon="['fa', 'info-circle']"/>
                </nuxt-link>
                <a class="btn btn-danger" v-if="data.item.manufacturerUsername == manufacturerUsername"
                   @click="remove(data.item.id)">
                  <fa :icon="['fas', 'trash']"/>
                </a>
              </template>
            </b-table>
            <b-pagination
              v-model="tables.sheets.currentPage"
              :total-rows="tables.sheets.rows"
              :per-page="tables.perPage"
              align="fill"
              limit="10"
              aria-controls="table-sheets"
            ></b-pagination>
          </b-card-body>
        </b-collapse>
      </b-card>
    </div>
  </div>
</template>
<script>
export default {
  name: "family", // ignored because the name is forced as the folder name
  data: function () {
    return {
      tables: {
        profiles: {
          currentPage: 1,
          rows: 0,
          filter: null
        },
        sheets: {
          currentPage: 1,
          rows: 0,
          filter: null
        },
        perPage: 5
      },
      profiles: [],
      profilesFields: [
        {
          key: 'id',
          label: 'ID'
        }, {
          key: 'familyName',
          label: 'Family Name'
        }, {
          key: 'materialName',
          label: 'Material Name'
        }, {
          key: 'weffP',
          label: 'Weff_P'
        }, {
          key: 'weffN',
          label: 'Weff_N'
        }, {
          key: 'ar',
          label: 'AR'
        }, {
          key: 'created',
          label: 'Created Date',
          formatter: (value) => this.$moment(value).format('L LT')
        }, {
          key: 'updated',
          label: 'Updated Date',
          formatter: (value) => value ? this.$moment(value).format('L LT') : ''
        },
        'actions'
      ],
      sheets: [],
      sheetsFields: [
        {
          key: 'id',
          label: 'ID'
        }, {
          key: 'familyName',
          label: 'Family Name'
        }, {
          key: 'materialName',
          label: 'Material Name'
        }, {
          key: 'thickness',
          label: 'Thickness'
        }, {
          key: 'created',
          label: 'Created Date',
          formatter: (value) => this.$moment(value).format('L LT')
        }, {
          key: 'updated',
          label: 'Updated Date',
          formatter: (value) => value ? this.$moment(value).format('L LT') : ''
        },
        'actions'
      ],
      manufacturerUsername: this.$auth.user.sub,
      items: [{
        text: 'Dashboard',
        to: {name: 'dashboard'}
      }, {
        text: 'Materials',
        active: true
      }]
    }
  },
  mounted() {
    this.$axios.$get(`/api/materials/`)
      .then((materials) => {
        for (const material of materials) {
          if (material.profile) {
            this.profiles.push({
              id: material.id,
              familyName: material.family.name,
              materialName: material.name,
              weffP: material.profile.weff_p,
              weffN: material.profile.weff_n,
              ar: material.profile.ar,
              created: material.created,
              updated: material.updated,
              manufacturerUsername: material.manufacturerUsername
            });
          } else if (material.sheet) {
            this.sheets.push({
              id: material.id,
              familyName: material.family.name,
              materialName: material.name,
              thickness: material.sheet.thickness,
              created: material.created,
              updated: material.updated,
              manufacturerUsername: material.manufacturerUsername
            });
          }
        }

        this.tables.profiles.rows = this.profiles.length
        this.tables.sheets.rows = this.sheets.length
      });
  },
  methods: {
    remove(id) {
      this.$axios.$delete(`/api/materials/${id}`).then(r => {
        this.$toast.success('Material deleted with success').goAway(1500)
        if (this.sheets.some(s => s.id == id)) {
          this.sheets = this.sheets.filter(i => i.id != id);
        } else if (this.profiles.some(p => p.id == id)) {
          this.profiles = this.profiles.filter(i => i.id != id);
        }
      }).catch(reason => {
        this.$toast.error('An error occurred while trying to delete.').goAway(1500)
      })
    },
    onFilteredProfiles(filteredItems) {
      this.tables.profiles.rows = filteredItems.length
      this.tables.profiles.currentPage = 1
    },
    onFilteredSheets(filteredItems) {
      this.tables.sheets.rows = filteredItems.length
      this.tables.sheets.currentPage = 1
    }
  }
}
</script>
<style scoped>

</style>
