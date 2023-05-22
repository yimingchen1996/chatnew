<template>
  <div>
		<div id="app" class="chat-container">
			<div class="chat-messages">
				<div v-for="message in chatMessages" :key="message.id" class="message-container">
					<div v-if="message.role === 'user'" class="user-message">{{ message.content }}</div>
					<div v-if="message.role === 'assistant'" class="assistant-message">{{ message.content }}</div>
				</div>
			</div>
		</div>	
    <div class="input-container">
      <input v-model="userMessage" type="text" placeholder="Enter a message" class="input-field" @keydown.enter="sendMessage">

      <button @click="sendMessage" class="send-button">Send</button>
    </div>
  </div>
</template>

<style>
.chat-container {
	height: 100%;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: space-between;
	background-color: #f2f2f2;
	padding: 20px;
	box-sizing: border-box;
}
.chat-messages {
  max-height: 500px;
  overflow-y: auto;
  padding: 10px;
  width: 400px;

}

.message-container {
  margin-bottom: 10px;
}

.user-message {
  background-color: #e0e0e0;
  color: #000;
  padding: 10px;
  border-radius: 8px;
}

.assistant-message {
  background-color: #007bff;
  color: #fff;
  padding: 10px;
  border-radius: 8px;
}

.input-container {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;

}
.input-wrapper {
	display: flex;
	align-items: center;
	padding: 10px;
}

.input-wrapper input {
	flex-grow: 1;
	padding: 10px;
	border: none;
	border-radius: 5px;
	background-color: #f2f2f2;
	box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);
}

.input-field {
  flex: 1;
  padding: 8px;
  border-radius: 8px;
  border: 1px solid #ccc;
}

.send-button {
  background-color: #007bff;
  color: #fff;
  padding: 8px 16px;
  border: none;
  border-radius: 8px;
  margin-left: 10px;
  cursor: pointer;
}

.send-button:hover {
  background-color: #0056b3;
}
</style>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      chatMessages: [],
      userMessage: ''
    };
  },
  methods: {
    async sendMessage() {
      const apiKey = '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';
      const apiUrl = 'https://api.openai.com/v1/chat/completions';

      axios.defaults.headers.common['Authorization'] = `Bearer ${apiKey}`;

      try {
        // 发送用户输入的消息
        this.chatMessages.push({ role: 'user', content: this.userMessage });

        const response = await axios.post(apiUrl, {
          model: 'gpt-3.5-turbo',
          messages: this.chatMessages,
          temperature: 0.7
        });

        // 添加助手的回复到聊天消息
        const assistantReply = response.data.choices[0];
        this.chatMessages.push({
          role: 'assistant',
          content: assistantReply.message.content
        });

        // 清空用户输入
        this.userMessage = '';

        // 滚动到底部
        this.$nextTick(() => {
          const chatContainer = document.querySelector('.chat-messages');
          chatContainer.scrollTop = chatContainer.scrollHeight;
        });
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>
