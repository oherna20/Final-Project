<template>
    <class class="content">
      <TicketBooking />
    </class>
  </template>
  
  <script>
  import TicketBooking from './TicketBooking.vue';
  
  export default {
    name: 'Home' ,
    components: { TicketBooking }
  }
  </script>
  <style>
  .content{
    padding:50px 0;
  }
  .content h1{
      color: #fff;
      font-size: 38px;
      text-align: center;
      letter-spacing: 1px;
      text-transform: uppercase;
  }
  </style>