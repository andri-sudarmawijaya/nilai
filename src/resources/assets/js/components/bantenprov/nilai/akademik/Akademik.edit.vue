<template>
  <div class="card">
    <div class="card-header">
      <i class="fa fa-table" aria-hidden="true"></i> Edit Akademik

      <ul class="nav nav-pills card-header-pills pull-right">
        <li class="nav-item">
          <button class="btn btn-primary btn-sm" role="button" @click="back">
            <i class="fa fa-arrow-left" aria-hidden="true"></i>
          </button>
        </li>
      </ul>
    </div>

    <div class="card-body">
      <vue-form class="form-horizontal form-validation" :state="state" @submit.prevent="onSubmit">

        <div class="form-row mt-4">
          <div class="col-md">
            <validate tag="div">
            <label for="siswa_id">Nama Siswa</label>
            <v-select name="siswa_id" v-model="model.siswa" :options="siswa" class="mb-4"></v-select>

            <field-messages name="siswa_id" show="$invalid && $submitted" class="text-danger">
              <small class="form-text text-success">Looks good!</small>
              <small class="form-text text-danger" slot="required">Nama Siswa is a required field</small>
            </field-messages>
            </validate>
          </div>
        </div>  

        <validate tag="div">
          <div class="form-group">
            <label for="model-bahasa_indonesia">Bahasa Indonesia</label>
            <input type="text" class="form-control" id="model-bahasa_indonesia" v-model="model.bahasa_indonesia" name="bahasa_indonesia" placeholder="Bahasa Indonesia" required>
            <field-messages name="bahasa_indonesia" show="$invalid && $submitted" class="text-danger">
              <small class="form-text text-success">Looks good!</small>
              <small class="form-text text-danger" slot="required">Bahasa Indonesia is a required field</small>
            </field-messages>
          </div>
        </validate>

        <validate tag="div">
          <div class="form-group">
            <label for="model-bahasa_inggris">Bahasa Inggris</label>
            <input type="text" class="form-control" id="model-bahasa_inggris" v-model="model.bahasa_inggris" name="bahasa_inggris" placeholder="Bahasa Inggris" required>
            <field-messages name="bahasa_inggris" show="$invalid && $submitted" class="text-danger">
              <small class="form-text text-success">Looks good!</small>
              <small class="form-text text-danger" slot="required">Bahasa Inggris is a required field</small>
            </field-messages>
          </div>
        </validate>

        <validate tag="div">
          <div class="form-group">
            <label for="model-matematika">Matematika</label>
            <input type="text" class="form-control" id="model-matematika" v-model="model.matematika" name="matematika" placeholder="Matematika" required>
            <field-messages name="matematika" show="$invalid && $submitted" class="text-danger">
              <small class="form-text text-success">Looks good!</small>
              <small class="form-text text-danger" slot="required">Matematika is a required field</small>
            </field-messages>
          </div>
        </validate>

        <validate tag="div">
          <div class="form-group">
            <label for="model-ipa">IPA</label>
            <input type="text" class="form-control" id="model-ipa" v-model="model.ipa" name="ipa" placeholder="IPA" required>
            <field-messages name="ipa" show="$invalid && $submitted" class="text-danger">
              <small class="form-text text-success">Looks good!</small>
              <small class="form-text text-danger" slot="required">IPA is a required field</small>
            </field-messages>
          </div>
        </validate>

        <div class="form-row mt-4">
          <div class="col-md">
            <validate tag="div">
            <label for="user_id">Username</label>
            <v-select name="user_id" v-model="model.user" :options="user" class="mb-4"></v-select>

            <field-messages name="user_id" show="$invalid && $submitted" class="text-danger">
              <small class="form-text text-success">Looks good!</small>
              <small class="form-text text-danger" slot="required">Username is a required field</small>
            </field-messages>
            </validate>
          </div>
        </div>

        <div class="form-row mt-4">
          <div class="col-md">            
            <button type="submit" class="btn btn-primary">Submit</button>

            <button type="reset" class="btn btn-secondary" @click="reset">Reset</button>            
          </div>
        </div>
        
      </vue-form>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    axios.get('api/akademik/' + this.$route.params.id + '/edit')
      .then(response => {
        if (response.data.status == true) {
          this.model.user = response.data.user;
          this.model.siswa = response.data.siswa;
          this.model.bahasa_indonesia  = response.data.akademik.bahasa_indonesia;
          this.model.bahasa_inggris  = response.data.akademik.bahasa_inggris;
          this.model.matematika  = response.data.akademik.matematika;
          this.model.ipa  = response.data.akademik.ipa;
        } else {
          alert('Failed');
        }
      })
      .catch(function(response) {
        alert('Break');
        window.location.href = '#/admin/akademik';
      }),

      axios.get('api/akademik/create')
      .then(response => {           
          response.data.siswa.forEach(element => {
            this.siswa.push(element);
          });
          response.data.user.forEach(element => {
            this.user.push(element);
          });
      })
      .catch(function(response) {
        alert('Break');
      })
  },
  data() {
    return {
      state: {},
      model: {
        siswa: "",
        bahasa_indonesia: "",
        bahasa_inggris: "",
        matematika: "",
        ipa: "",
        user: ""
      },
      user: [],
      siswa: []
    }
  },
  methods: {
    onSubmit: function() {
      let app = this;

      if (this.state.$invalid) {
        return;
      } else {
        axios.put('api/akademik/' + this.$route.params.id, {
            siswa_id: this.model.siswa.id,
            bahasa_indonesia: this.model.bahasa_indonesia,
            bahasa_inggris: this.model.bahasa_inggris,
            matematika: this.model.matematika,
            ipa: this.model.ipa,
            user_id: this.model.user.id
          })
          .then(response => {
            if (response.data.status == true) {
              if(response.data.message == 'success'){
                alert(response.data.message);
                app.back();
              }else{
                alert(response.data.message);
              }
            } else {
              alert(response.data.message);
            }
          })
          .catch(function(response) {
            alert('Break ' + response.data.message);
          });
      }
    },
    reset() {
      axios.get('api/akademik/' + this.$route.params.id + '/edit')
        .then(response => {
          if (response.data.status == true) {
            this.model.bahasa_indonesia = response.data.akademik.bahasa_indonesia;
            this.model.bahasa_inggris = response.data.akademik.bahasa_inggris;
            this.model.matematika = response.data.akademik.matematika;
            this.model.ipa = response.data.akademik.ipa;
          } else {
            alert('Failed');
          }
        })
        .catch(function(response) {
          alert('Break ');
        });
    },
    back() {
      window.location = '#/admin/akademik';
    }
  }
}
</script>
