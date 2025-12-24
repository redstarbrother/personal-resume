<template>
  <div class="profile-card">
    <div class="avatar-container">
      <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Felix" alt="Avatar" class="avatar" />
    </div>
    <div class="profile-info">
      <h2 class="name">I'm N0tsss <span class="cursor">|</span></h2>
      <div class="divider"></div>
      <div class="description">
        <p>一条咸鱼 🎣</p>
        <p>爱好篮球，计算机</p>
        <p>瞎折腾浪费时间最在行</p>
        <p>最爱🍦</p>
      </div>
    </div>
    <div class="card-footer">
      <a href="#" class="link">关于</a>
      <a href="#" class="link">联系</a>
    </div>
  </div>
</template>

<script setup>
</script>

<style scoped>
/* Profile Card */
.profile-card {
  background: white;
  width: 300px;
  border-radius: 10px;
  padding: 0 0 20px 0;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
  text-align: center;
  position: relative;
  display: flex;
  flex-direction: column;
  height: 400px;
}

.avatar-container {
  width: 100px;
  height: 100px;
  margin: -50px auto 20px; /* Pull up */
  background: white; /* Or matching background */
  border-radius: 50%;
  padding: 5px;
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
}

.avatar {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  object-fit: cover;
}

.profile-info {
  flex: 1;
  padding: 0 20px;
}

.name {
  font-size: 1.5rem;
  font-weight: normal;
  margin-bottom: 10px;
  letter-spacing: 2px;
  font-family: "Menlo", "Monaco", "Consolas", "Courier New", monospace;
}

.cursor {
  animation: blink 1s step-end infinite;
}

@keyframes blink {
  50% { opacity: 0; }
}

.divider {
  height: 2px;
  background: #3498db; /* Blue line */
  width: 100%;
  margin: 10px 0 20px;
}

.description p {
  margin: 8px 0;
  color: #666;
  font-size: 0.95rem;
}

.card-footer {
  display: flex;
  justify-content: space-between;
  padding: 0 40px;
  margin-top: auto;
}

.link {
  text-decoration: none;
  color: #999;
  font-size: 0.9rem;
  transition: color 0.3s;
}

.link:hover {
  color: #333;
}
</style>
