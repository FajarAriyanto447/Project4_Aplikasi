<template>
  <div>
    <div class="ngent">
      <p style="text-align: center">Selamat Datang Admin...</p>
      <br /><br />
      <div class="bungkus">
        <div class="op">
          <p>From Peminjaman Buku</p>

          <form @submit.prevent="add">
            <span>Nama Sewa </span><br /><input
              type="text"
              v-model="form.nama"
              placeholder="Masukan Nama Sewa"
              required
            /><br /><br />

            <span>Judul Buku </span><br /><input
              type="text"
              v-model="form.judulBuku"
              placeholder="Masukan Judul Buku"
              required
            /><br /><br />

            <span>Tanggal Pinjam </span><br /><input
              type="date"
              v-model="form.tanggalPinjam"
              placeholder="Masukan Tanggal Pinjam"
              required
            /><br /><br />

            <span>Tanggal Pengembalian</span><br /><input
              type="date"
              v-model="form.tanggalPengembalian"
              placeholder="Masukan Tanggal Pengembalian"
              required
            /><br /><br />

            <button class="c" type="submit" v-show="!updateSubmit">
              Add Peminjaman</button
            ><br />
            <button
              class="c"
              type="button"
              v-show="updateSubmit"
              @click="update(form)"
            >
              Add Perpanjangan
            </button>
          </form>
          <br /><br />
        </div>
        <div class="pp">
          <p style="margin-left: 50px">Table Daftar Buku</p>
          <form action="/action_page.php">
            <label for="cars" style="margin-left: 49px"
              >Batas Max Waktu Peminjaman
            </label>
            <select name="cars" id="cars">
              <option value="volvo">5</option>
              <option value="saab">10</option>
              <option value="opel">15</option>
              <option value="audi">20</option>
            </select>
            <label for="cars"> Hari</label>
            <br /><br />
          </form>

          <table style="width: 100%">
            <tr>
              <th>No</th>
              <th>Nama Sewa</th>
              <th>Judul Buku</th>
              <th>Tanggal Pinjam</th>
              <th>Tanggal Pengembalian</th>
              <th>Aksi</th>
            </tr>
            <tr v-for="user in users" :key="user.id">
              <td>{{ user.id }}</td>
              <td>{{ user.nama }}</td>
              <td>{{ user.judulBuku }}</td>
              <td>{{ user.tanggalPinjam }}</td>
              <td>{{ user.tanggalPengembalian }}</td>
              <td>
                <button class="a" @click="edit(user)">Perpanjangan</button>
                <br /><br />
                <button class="b" @click="del(user)">Kembali</button>
              </td>
            </tr>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import axios from "axios";
export default {
  data() {
    return {
      form: {
        id: "",
        name: "",
      },
      users: "",
      updateSubmit: false,
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      axios
        .get("http://localhost:3000/users")
        .then((res) => {
          this.users = res.data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    del(user) {
      axios.delete("http://localhost:3000/users/" + user.id).then((res) => {
        this.load();
        let index = this.users.indexOf(form.name);
        this.users.splice(index, 1);
      });
    },
    add() {
      axios.post("http://localhost:3000/users/", this.form).then((res) => {
        this.load();
        this.form.nama = "";
        this.form.judulBuku = "";
        this.form.tanggalPinjam = "";
        this.form.tanggalPengembalian = "";
      });
    },
    edit(user) {
      this.updateSubmit = true;
      this.form.id = user.id;
      this.form.nama = user.nama;
      this.form.judulBuku = user.judulBuku;
      this.form.tanggalPinjam = user.tanggalPinjam;
      this.form.tanggalPengembalian = user.tanggalPengembalian;
    },
    update(form) {
      return axios
        .put("http://localhost:3000/users/" + form.id, {
          id: this.form.id,
          nama: this.form.nama,
          judulBuku: this.form.judulBuku,
          tanggalPinjam: this.form.tanggalPinjam,
          tanggalPengembalian: this.form.tanggalPengembalian,
        })
        .then((res) => {
          this.load();
          this.form.id = "";
          this.form.nama = "";
          this.form.judulBuku = "";
          this.form.tanggalPinjam = "";
          this.form.tanggalPengembalian = "";
          this.updateSubmit = false;
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
table {
  border: 1px solid black;
  background-color: whitesmoke;
  margin-left: 50px;
}
input {
  width: 90%;
}
td {
  border: none;
}
.a {
  background-color: rgb(19, 245, 19);
  color: whitesmoke;
  border: none;
  padding: 5px;
  border-radius: 3px;
}
.b {
  background-color: rgb(255, 0, 0);
  color: whitesmoke;
  border: none;
  padding: 5px;
  border-radius: 3px;
  width: 93px;
}
.c {
  border: none;
  background-color: turquoise;
  padding-left: 48px;
  padding-right: 48px;
}
div {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 14px;
}
p {
  font-size: 20px;
}
.ngent {
  border-radius: 1px;
  background-color: lightblue;
  margin: 60px;
  padding: 20px;
}
.bungkus {
  display: flex;
  margin-left: 20px;
}
.pp {
  border: none;
}
button:hover {
  background-color: blue;
}
</style>
