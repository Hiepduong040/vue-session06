<template>
  <div>
    <input
      v-model="valueInput"
      type="text"
      placeholder="Thêm nội dung công việc"
    />
    <button @click="addJob">Thêm</button>
    <div class="list">
      <h3>Danh sách công việc</h3>
      <ul>
        <li v-for="job in jobs" :key="job.id">
          <input type="checkbox" @change="handleChange(job.id)" :checked="job.status" />
          <span v-if="job.status" class="success">{{ job.name }}</span>
          <span v-else class="unsuccess">{{ job.name }}</span>
          <button @click="handleDelete(job.id)">Xóa</button>
        </li>
      </ul>
    </div>
    <p>Số công việc hoàn thành: {{ completeJob }}</p>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from "vue";

const valueInput = ref("");
// Tạo mảng chứa các công việc
const jobs = reactive(JSON.parse(localStorage.getItem("jobs")) || []);

// Hàm thêm công việc
const addJob = () => {
  if (valueInput.value.trim()) {
    let job = {
      name: valueInput.value,
      status: false, // Mặc định trạng thái là chưa hoàn thành
      id: Math.ceil(Math.random() * 1000000000),
    };
    jobs.push(job);
    // Lưu danh sách công việc vào localStorage
    localStorage.setItem("jobs", JSON.stringify(jobs));
    valueInput.value = ""; // Reset ô nhập
  } else {
    alert("Bạn chưa nhập nội dung công việc!");
  }
};

// Hàm tính toán số lượng công việc hoàn thành
const completeJob = computed(() => {
  return jobs.reduce((accumulator, currentValue) => {
    return accumulator + (currentValue.status ? 1 : 0);
  }, 0);
});

// Hàm cập nhật trạng thái của từng công việc
const handleChange = (id) => {
  const index = jobs.findIndex((item) => item.id === id);
  if (index !== -1) {
    jobs[index].status = !jobs[index].status; // Đảo ngược trạng thái
    // Lưu danh sách đã cập nhật vào localStorage
    localStorage.setItem("jobs", JSON.stringify(jobs));
  }
};

// Hàm xóa công việc
const handleDelete = (id) => {
  const index = jobs.findIndex((item) => item.id === id);
  if (index !== -1) {
    jobs.splice(index, 1); // Xóa công việc khỏi mảng
    // Lưu danh sách đã cập nhật vào localStorage
    localStorage.setItem("jobs", JSON.stringify(jobs));
  }
};
</script>

<style>
.success {
  text-decoration: line-through;
}

</style>
