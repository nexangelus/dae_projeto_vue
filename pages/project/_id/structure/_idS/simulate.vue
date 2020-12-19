<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Simulate</h2>
    </div>
    <div class="accordion" role="tablist">
      <b-card no-body class="mb-1">
        <b-card-header header-tag="header" class="p-1" role="tab">
          <b-button block v-b-toggle.accordion-profile variant="info">Profiles</b-button>
        </b-card-header>
        <b-card-body>
          <b-form-input
            id="filter-input-profiles"
            v-model="tables.profiles.filter"
            type="search"
            placeholder="Type to Search"
          ></b-form-input>
          <b-table striped hover :items="profiles" :fields="profilesFields"
                   :filter="tables.profiles.filter" @filtered="onFilteredProfiles">
            <template #cell(actions)="data">
              <button class="btn btn-primary" v-on:click="simulate(data.item.id)">
                <fa :icon="['fas', 'cogs']"/>
              </button>
            </template>
            <template #cell(simulate)="simulate">
              <fa :icon="['fa', 'check-circle']" v-if="simulate.item.simulated == true"/>
              <fa :icon="['fa', 'times-circle']" v-if="simulate.item.simulated == false"/>
            </template>
            <template #cell(selected)="selected">
              <b-checkbox v-model="selected.item.selected"  @change="deciderSecound(selected.item.id, $event)"/>
            </template>
          </b-table>
        </b-card-body>
      </b-card>

      <b-card no-body class="mb-1">
        <b-card-header header-tag="header" class="p-1" role="tab">
          <b-button block v-b-toggle.accordion-sheet variant="info">Sheets</b-button>
        </b-card-header>
        <b-card-body>
          <b-form-input
            id="filter-input-profiles"
            v-model="tables.sheets.filter"
            type="search"
            placeholder="Type to Search"
          ></b-form-input>
          <b-table striped hover :items="sheets" :fields="sheetsFields"
                   :filter="tables.sheets.filter" @filtered="onFilteredSheets">
            <template #cell(actions)="data">
              <nuxt-link class="btn btn-primary" :to="`/materials/${data.item.id}`">
                <fa :icon="['fa', 'info-circle']"/>
              </nuxt-link>
              <a class="btn btn-danger" v-if="data.item.manufacturerUsername == manufacturerUsername"
                 @click="remove(data.item.id)">
                <fa :icon="['fas', 'trash']"/>
              </a>
            </template>
            <template #cell(selected)="selected">
              <b-checkbox v-model="selected.item.selected" @change="deciderSecound(selected.item.id, $event)"/>
            </template>
          </b-table>
        </b-card-body>
      </b-card>
    </div>
    <button class="btn btn-primary">Finish</button>
  </div>
</template>
<script>
export default {
  auth: false,
  data() {
    return {
      idProject: this.$route.params.id,
      items: [{
        text: 'Dashboard',
        to: {name: 'dashboard'}
      }, {
        text: 'Project Details',
        to: `/project/${this.$route.params.id}`
      }, {
        text: 'Structure Details',
        to: `/project/${this.$route.params.id}/structure/${this.$route.params.idS}`
      }, {
        text: 'Add Materials',
        active: true
      }],
      profiles: [],
      profilesFields: [
        {
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
        },
        'actions',
        'simulate',
        'selected'
      ],
      sheets: [],
      sheetsFields: [
        {
          key: 'familyName',
          label: 'Family Name'
        }, {
          key: 'materialName',
          label: 'Material Name'
        }, {
          key: 'thickness',
          label: 'Thickness'
        },
        'selected'
      ],
      tables: {
        profiles: {
          filter: null
        },
        sheets: {
          filter: null
        }
      },
      structure: [],
      material: {}
    };
  },
  computed: {
    id() {
      return this.$route.params.id
    },
    idS() {
      return this.$route.params.idS
    }
  },
  mounted() {
    this.$axios.$get(`/api/projects/${this.id}/structures/${this.idS}`).then((response) => {
        this.structure = response
        this.material = response.materials.map(m=>m.id)
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
              manufacturerUsername: material.manufacturerUsername,
              simulated: null,
              selected: this.deciderFirst(material.id)
            });
          } else if (material.sheet) {
            this.sheets.push({
              id: material.id,
              familyName: material.family.name,
              materialName: material.name,
              thickness: material.sheet.thickness,
              created: material.created,
              updated: material.updated,
              manufacturerUsername: material.manufacturerUsername,
              selected: this.deciderFirst(material.id)
            });
          }
        }
      })
      })  
  },
  methods: {
    deciderFirst(id) {
        return this.material.includes(id)
    },
    deciderSecound(id, event){
        if(event){
          this.save(id)
        }else {
          this.remove(id)
        }
    },
    save(id) {
      this.$axios.post(`/api/projects/${this.id}/structures/${this.idS}/material/${id}`).then((response) => {
          if (response.status == 200) {
            this.$toast.success(response.data).goAway(4000)
          } else {
            this.$toast.danger(response.data).goAway(4000)
          }
        });
    },
    remove(id) {
      this.$axios.delete(`/api/projects/${this.id}/structures/${this.idS}/material/${id}`).then((response) => {
          if (response.status == 200) {
            this.$toast.success(response.data).goAway(4000)
          } else {
            this.$toast.danger(response.data).goAway(4000)
          }
        });
    },
    simulate(materialId) {
      let material = this.profiles.find(p => p.id == materialId)
      this.$axios.post(`/api/projects/${this.id}/structures/${this.idS}/simulate`,
        material
      ).then((response) => {
        if (response.status == 200) {
          material.simulated = response.data.value
          if (response.data.value) {
            this.$toast.success("Can use material").goAway(4000)

          } else {
            this.$toast.danger("Can't use material").goAway(4000)
          }
        } else {
          this.$toast.danger("Something went wrong").goAway(4000)
        }
      });
    },
    onFilteredProfiles(filteredItems) {
      this.tables.profiles.rows = filteredItems.length
      this.tables.profiles.currentPage = 1
    },
    onFilteredSheets(filteredItems) {
      this.tables.sheets.rows = filteredItems.length
      this.tables.sheets.currentPage = 1
    }
  },
};
</script>
