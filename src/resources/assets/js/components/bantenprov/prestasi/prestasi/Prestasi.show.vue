<template>
  <div class="card">
    <div class="card-header">
      <i class="fa fa-table" aria-hidden="true"></i> Prestasi 

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
            <b>Nama Siswa :</b> {{ model.siswa.nama_siswa }}
          </div>
        </div>

        <div class="form-row mt-4">
          <div class="col-md">
            <b>Master Prestasi :</b> {{ model.master_prestasi.juara }}
          </div>
        </div>

        <div class="form-row mt-4">
          <div class="col-md">
            <b>Nama Lomba :</b> {{ model.nama_lomba }}
          </div>
        </div>
        
      </vue-form>
    </div>
       <div class="card-footer text-muted">
        <div class="row">
          <div class="col-md">
            <b>Username :</b> {{ model.user.name }}
          </div>
          <div class="col-md">
            <div class="col-md text-right">Dibuat : {{ model.created_at }}</div>
            <div class="col-md text-right">Diperbaiki : {{ model.updated_at }}</div>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  mounted() {
    axios.get('api/prestasi/' + this.$route.params.id)
      .then(response => {
        if (response.data.status == true) {
          this.model.user = response.data.user;
          this.model.master_prestasi = response.data.master_prestasi;
          this.model.siswa = response.data.siswa;
          this.model.nama_lomba = response.data.prestasi.nama_lomba;
          this.model.created_at = response.data.prestasi.created_at;
          this.model.updated_at = response.data.prestasi.updated_at;
        } else {
          alert('Failed');
        }
      })
      .catch(function(response) {
        alert('Break');
        window.location.href = '#/admin/prestasi';
      })

  },
  data() {
    return {
      state: {},
      model: {
        user: "",
        master_prestasi: "",
        siswa: "",
        nama_lomba: "",
        created_at:       "",
        updated_at:       "",
      },
      user: [],
      master_prestasi: [],
      siswa: []
    }
  },
  methods: {
    onSubmit: function() {
      let app = this;

      if (this.state.$invalid) {
        return;
      } else {
        axios.put('api/prestasi/' + this.$route.params.id, {
            master_prestasi_id: this.model.master_prestasi.id,
            siswa: this.model.siswa.id,
            nama_lomba: this.model.old_label,
            user_id: this.model.prestasi.id
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
      axios.get('api/prestasi/' + this.$route.params.id + '/edit')
        .then(response => {
          if (response.data.status == true) {
            this.model.label = response.data.prestasi.label;
            this.model.description = response.data.prestasi.description;
          } else {
            alert('Failed');
          }
        })
        .catch(function(response) {
          alert('Break ');
        });
    },
    back() {
      window.location = '#/admin/prestasi';
    }
  }
}
</script>
