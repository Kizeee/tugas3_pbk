<template>
  <div id="app">
    <h1>Daftar Kegiatan</h1>
    <div class="input-container">
      <input type="text" v-model="newActivity" placeholder="Tambahkan kegiatan baru...">
      <button @click="addActivity">Tambah</button>
    </div>

    <ul>
      <li v-for="(activity, index) in filteredActivities" :key="index" class="activity-item">
        <span v-if="index !== editingIndex" :class="{ completed: activity.completed }">{{ activity.name }}</span>
        <input v-else type="text" v-model="editedActivityName" @keyup.enter="saveEdit" @keyup.esc="cancelEdit">
        <div>
          <button @click="toggleCompletion(index)" class="action-button">{{ activity.completed ? 'Batal Checklist' : 'Checklist' }}</button>
          <button @click="editActivity(index)" class="action-button">{{ index === editingIndex ? 'Simpan' : 'Edit' }}</button>
          <button @click="removeActivity(index)" class="action-button">Hapus</button>
        </div>
      </li>
    </ul>

    <div class="filter-container">
      <input type="checkbox" id="showOnlyPending" v-model="showOnlyPending">
      <label for="showOnlyPending">Tampilkan hanya kegiatan yang belum selesai</label>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newActivity: '',
      editedActivityName: '',
      editingIndex: -1,
      activities: [],
      showOnlyPending: false
    };
  },
  methods: {
    addActivity() {
      if (this.newActivity.trim() !== '') {
        this.activities.push({ name: this.newActivity, completed: false });
        this.newActivity = '';
      }
    },
    removeActivity(index) {
      this.activities.splice(index, 1);
    },
    toggleCompletion(index) {
      this.activities[index].completed = !this.activities[index].completed;
    },
    editActivity(index) {
      this.editedActivityName = this.activities[index].name;
      this.editingIndex = index;
    },
    saveEdit() {
      if (this.editedActivityName.trim() !== '') {
        this.activities[this.editingIndex].name = this.editedActivityName;
        this.cancelEdit();
      }
    },
    cancelEdit() {
      this.editedActivityName = '';
      this.editingIndex = -1;
    }
  },
  computed: {
    filteredActivities() {
      if (this.showOnlyPending) {
        return this.activities.filter(activity => !activity.completed);
      }
      return this.activities;
    }
  }
};
</script>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  color: #cccddb;
}

.input-container {
  margin-bottom: 10px;
  display: flex;
  flex-direction: column;
}

.input-container input {
  padding: 8px;
  font-size: 16px;
  margin-bottom: 10px;
}

.input-container button {
  padding: 8px 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  cursor: pointer;
  font-size: 16px;
}

ul {
  list-style-type: none;
  padding: 0;
}

.activity-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #ccc;
  padding: 10px 0;
}

.activity-item:last-child {
  border-bottom: none;
}

.completed {
  text-decoration: line-through;
  color: #777;
}

.filter-container {
  margin-top: 20px;
}

.action-button {
  padding: 6px 12px;
  margin-left: 5px;
  background-color: #333;
  color: #fff;
  border: none;
  cursor: pointer;
  font-size: 14px;
}

.action-button:hover {
  background-color: #555;
}
</style>
