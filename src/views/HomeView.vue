<template>
  <div class="container mt-4">
    <div class="card shadow-sm">
      <div class="card-header text-center bg-primary text-white">
        <h3 class="my-0">Daftar Pembelian Baju</h3>
      </div>
      <div class="card-body">
        <!-- Bagian untuk menambah aktivitas -->
        <div class="mb-3">
          <div class="input-group">
            <input
              type="text"
              class="form-control"
              v-model="newActivity.name"
              placeholder="Nama Barang"
            />
            <input
              type="date"
              class="form-control"
              v-model="newActivity.date"
            />
            <select
              class="form-select"
              v-model="newActivity.status"
            >
              <option value="Belum Selesai">Belum Selesai</option>
              <option value="Selesai">Selesai</option>
            </select>
            <button
              class="btn btn-success"
              @click="addOrUpdateActivity"
            >
              {{ isEditing ? 'Update' : 'Tambah' }}
            </button>
          </div>
        </div>

        <!-- Filter aktivitas berdasarkan status -->
        <div class="form-group mb-3">
          <label for="filter" class="form-label">Filter:</label>
          <select
            id="filter"
            class="form-select"
            v-model="filter"
          >
            <option value="">Semua</option>
            <option value="Selesai">Selesai</option>
            <option value="Belum Selesai">Belum Selesai</option>
          </select>
        </div>

        <!-- Daftar aktivitas -->
        <table class="table table-hover table-striped">
          <thead>
            <tr>
              <th>No</th>
              <th>Nama Barang</th>
              <th>Tanggal</th>
              <th>Status</th>
              <th>Tindakan</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="(activity, index) in filteredActivities"
              :key="index"
              :class="{ 'text-decoration-line-through': activity.completed }"
            >
              <td>{{ index + 1 }}</td>
              <td>{{ activity.name }}</td>
              <td>{{ activity.date }}</td>
              <td>
                <span
                  class="badge"
                  :class="activity.completed ? 'bg-success' : 'bg-warning'"
                >
                  {{ activity.status }}
                </span>
              </td>
              <td>
                <button
                  type="button"
                  class="btn btn-info btn-sm"
                  @click="editActivity(activity)"
                >
                  <i class="bi bi-pencil-square"></i> Edit
                </button>
                <button
                  type="button"
                  class="btn btn-danger btn-sm"
                  @click="removeActivity(index)"
                >
                  <i class="bi bi-trash"></i> Hapus
                </button>
              </td>
            </tr>
            <tr v-if="filteredActivities.length === 0">
              <td colspan="5" class="text-center">Tidak ada aktivitas</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue';

export default {
  name: 'Kegiatan',
  data() {
    return {
      activities: [],
      newActivity: {
        name: '',
        date: '',
        status: 'Belum Selesai',
        completed: false,
      },
      isEditing: false,
      editIndex: null,
      filter: '',
    };
  },
  methods: {
    addOrUpdateActivity() {
      if (this.isEditing && this.editIndex !== null) {
        this.activities[this.editIndex] = { ...this.newActivity };
        this.isEditing = false;
        this.editIndex = null;
      } else if (this.newActivity.name.trim() !== '' && this.newActivity.date.trim() !== '') {
        this.activities.push({ ...this.newActivity });
      }
      this.clearForm();
    },
    editActivity(activity) {
      this.newActivity = { ...activity };
      this.isEditing = true;
      this.editIndex = this.activities.indexOf(activity);
    },
    removeActivity(index) {
      this.activities.splice(index, 1);
    },
    toggleCompletion(activity) {
      activity.completed = !activity.completed;
      activity.status = activity.completed ? 'Selesai' : 'Belum Selesai';
    },
    clearForm() {
      this.newActivity = {
        name: '',
        date: '',
        status: 'Belum Selesai',
        completed: false,
      };
    },
  },
  computed: {
    filteredActivities() {
      if (this.filter === '') {
        return this.activities;
      } else if (this.filter === 'Selesai') {
        return this.activities.filter((activity) => activity.status === 'Selesai');
      } else {
        return this.activities.filter((activity) => activity.status === 'Belum Selesai');
      }
    },
  },
};
</script>

<style scoped>
.card {
  border-radius: 10px;
  border: 1px solid #ddd;
}

.table-striped tr.text-decoration-line-through {
  text-decoration: line-through;
}

.input-group {
  display: flex;
  flex-direction: row;
  gap: 10px;
}

.badge {
  font-size: 1em;
}

.btn {
  margin-left: 10px;
}

.card-header {
  padding: 15px;
}

.card {
  margin-bottom: 20px;
  padding: 20px;
}
</style>
