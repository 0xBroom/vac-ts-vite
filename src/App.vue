<template>
  <vue-advanced-chat height="calc(100vh - 20px)" :current-user-id="currentUserId" :rooms="JSON.stringify(rooms)"
    :rooms-loaded="true" :messages-loaded="messagesLoaded" :messages="JSON.stringify(messages)"
    :room-actions="JSON.stringify(roomActions)" :theme="theme" @fetch-messages="fetchMessages($event.detail[0])"
    @send-message="sendMessage($event.detail[0])" />
</template>

<script lang="ts">
import { ref, onMounted } from 'vue';
import { register } from 'vue-advanced-chat';
import { Message } from "./interfaces/Message";

register();

export default {
  setup() {
    const currentUserId = ref('1234');
    const rooms = ref([
      {
        roomId: '1',
        roomName: 'Room 1',
        avatar: 'https://66.media.tumblr.com/avatar_c6a8eae4303e_512.pnj',
        users: [
          { _id: '1234', username: 'John Doe' },
          { _id: '4321', username: 'John Snow' }
        ]
      }
    ]);
    const messages = ref([] as Message[]);
    const roomActions = ref([
      { name: 'inviteUser', title: 'Invite User' },
      { name: 'removeUser', title: 'Remove User' },
      { name: 'deleteRoom', title: 'Delete Room' },
    ]);
    const messagesLoaded = ref(false);
    const theme = 'dark';

    // Function to fetch messages
    function fetchMessages(options: { reset?: boolean } = {}) {
      setTimeout(() => {
        if (options.reset) {
          messages.value = addMessages(true);
        } else {
          messages.value = [...addMessages(), ...messages.value];
          // Assuming you want to set messagesLoaded to true here
          // You can uncomment the following line if needed
          // messagesLoaded.value = true;
        }
        // this.addNewMessage(); // Uncomment this line if necessary
      });
    }

    // Function to add messages
    function addMessages(reset?: boolean) {
      const newMessages: Message[] = [];

      for (let i = 0; i < 30; i++) {
        newMessages.push({
          _id: reset ? i : messages.value.length + i,
          content: `${reset ? '' : 'paginated'} message ${i + 1}`,
          senderId: '4321',
          username: 'John Doe',
          date: '13 November',
          timestamp: '10:20',
        });
      }

      return newMessages;

      // Add the new messages to the existing messages
      // messages = reset ? newMessages : [...messages.value, ...newMessages];
    }

    // Function to send messages
    function sendMessage(message: Message) {
      const newMessage: Message = {
        _id: messages.value.length,
        content: message.content,
        senderId: currentUserId.value,
        username: "hannibal",
        timestamp: new Date().toString().substring(16, 21),
        date: new Date().toDateString()
      }
      messages.value = [...messages.value, newMessage]
    }

    // Simulate any asynchronous data loading if needed
    onMounted(async () => {
      // Example: Fetch data and update ref variables here
      // rooms.value = await fetchData();
      // messages.value = await fetchMessages();
    });

    return {
      currentUserId,
      rooms,
      messages,
      roomActions,
      messagesLoaded,
      theme,
      fetchMessages,
      sendMessage,
    };
  },
};


</script>

<style lang="scss">
body {
  font-family: 'Quicksand', sans-serif;
}
</style>