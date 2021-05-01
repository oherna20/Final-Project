<template>
    <div class="main">
      <h2>IMAX</h2>
      <div class="demo">
        <div id="seat-map">
          <div class="front">Screen</div>
            <div class="seatCharts-row" v-for="row in seats">
              <div class="seatCharts-cell seatCharts-space">{{row}}</div>
              <div v-for="col in seats" :row="row"  :col="col == 10 ? 0 : col"  @click="selectSeat"
              :class="['seatCharts-seat seatCharts-cell ', 
              { 'unavailable': (soldSeats.indexOf(Number(row.toString() + (col == 10 ? 0 : col).toString()) - 10 ) != -1),
                'selected': (selectedSeats.indexOf(Number(row.toString() + (col == 10 ? 0 : col).toString()) - 10 ) != -1),
                'available': (soldSeats.indexOf(Number(row.toString() + (col == 10 ? 0 : col).toString()) - 10) == -1) }]">
                {{col}}
              </div>
            </div>
        </div>
        <div class="booking-details">
          <ul class="book-left">
            <li>Movie </li>
            <li>Schedule </li>
            <li>Tickets</li>
            <li>Amount</li>
            <li>Location :</li>
          </ul>
          <ul class="book-right">
            <li>: Mortal Kombat</li>
            <li>: April 25, 12:00 A.M.</li>
            <li>: <span id="counter">{{counter}}</span></li>
            <li>: <b><span id="total">{{total}} <i>$6</i></span></b></li>
          </ul>
          <div class="clear"></div>
          <ul id="selected-seats" class="scrollbar scrollbar1" >
            <li v-for="seat in selectedSeats" >Row {{getRowAndSeat(seat).row}} Seat {{getRowAndSeat(seat).seat}}</li>
          </ul>   
          <button class="checkout-button book"  @click="book" >Purchase!</button> 
          <button class="checkout-button cancel"  @click="cancel" >Clear</button> 
          <div id="legend" class="seatCharts-legend">
            <ul class="seatCharts-legendList">
              <li class="seatCharts-legendItem">
              <div class="seatCharts-seat seatCharts-cell available"></div>
              <span class="seatCharts-legendDescription">Available</span></li><li class="seatCharts-legendItem"><div class="seatCharts-seat seatCharts-cell unavailable"></div><span class="seatCharts-legendDescription">Unavailable</span></li><li class="seatCharts-legendItem"><div class="seatCharts-seat seatCharts-cell selected"></div><span class="seatCharts-legendDescription">Selected</span></li></ul></div>
        </div>
        <div style="clear:both"></div>
        </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'TicketBooking',
  
    data(){
      return{
        counter: 0,
        total: 0,
        soldSeats: [],
        selectedSeats: [],
        ticket: 100,
        seats: [
          1, 2, 3, 4, 5, 6, 7, 8, 9, 10
        ]
      }
    },
  
    methods:{
      randomSeats(){
        while(this.soldSeats.length < 10){
          var randomnumber = Math.floor(Math.random()*100) + 1;
          if(this.soldSeats.indexOf(randomnumber) > -1) continue;
          this.soldSeats[this.soldSeats.length] = randomnumber;
        }
      },
  
      selectSeat(e){
        let row = e.currentTarget.getAttribute('row');
        let col = e.currentTarget.getAttribute('col');
        let seat = Number(row.toString() + (col == 10 ? 0 : col).toString()) - 10;
        let index = this.selectedSeats.indexOf(seat);
        if (index > -1) {
          this.selectedSeats.splice(index, 1);
        } else {
          this.selectedSeats.push(seat);
        }
        this.counter = this.selectedSeats.length;
        this.total = this.selectedSeats.length * this.ticket;
      },
  
      getRowAndSeat(num){
        return {
          row: Math.floor(num/10) + 1,
          seat: num % 10 == 0 ? 10 :  num % 10
        };
      },
  
      book(){
        if(this.selectedSeats.length) {
          this.notify();
        }
        this.soldSeats.push(...this.selectedSeats);
        this.counter = 0;
        this.total = 0;
        this.selectedSeats = [];
        
      },
  
      cancel(){
        this.selectedSeats = [];
        this.counter = 0;
        this.total = 0;
      },
  
      notify () {
        this.$popup({
          message         : 'Enjoy your movie!',
          color           : '#fff',
          backgroundColor : '#D88A04',
          delay           : 5
        })
      }  
    },
  
    created(){
      this.randomSeats();
      console.log(this.soldSeats);
    }
  }
  </script>
  
  <style>
  .main{
      width: 45%;
      margin: 45px auto;
      background:rgba(255, 255, 255, 0.88);
      padding: 30px 30px;
  }
  
  .main h2 {
      color: #000000;
      font-size: 28px;
      text-align: center;
      margin-bottom: 30px;
      text-transform: capitalize;
      font-weight: 500;
  }
  p.copy_rights {
      color: #fff;
      font-size: 14px;
      text-align: center;
  }
  p.copy_rights a{
    text-decoration:none;
    color:#fff;
  }
  p.copy_rights a:hover{
    text-decoration:underline;
  }
  
  /*-- movie ticket --*/
  #seat-map{
    float:left;
  }
  
  .front{
    margin: 5px 4px 45px 38px;
    background-color: #D88A04;
    color: #fff;
    text-align: center;
    padding: 9px 0;
    border-radius: 3px;
  }
  .booking-details {
    float: right;
    width: 38%;
  }
  .booking-details h3 {
    margin: 5px 5px 0 0;
    font-size: 16px;
  }
  .booking-details p {
    line-height: 1.5em;
    font-size: 18px;
    color: #D88A04;
    font-weight:600;
  }
  .booking-details p span{
    color: #000;
    font-size: 14px;
    font-weight:normal;
  }
  div.seatCharts-cell {
    color: #182C4E;
    height: 29px;
    width: 29px;
    line-height: 27px;
    margin: 3px;
    float: left;
    text-align: center;
    outline: none;
    font-size: 13px;
  }
  div.seatCharts-seat {
    color: #fff;
    cursor: pointer;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    border-radius: 3px;
  }
  div.seatCharts-row {
    height: 35px;
  }
  div.seatCharts-seat.available {
    background-color: #00B70C;
  }
  div.seatCharts-seat.focused, div.seatCharts-seat.available:hover {
    background-color: #D88A04;
    border: none;
  }
  div.seatCharts-seat.selected {
    background-color: #D88A04;
  }
  div.seatCharts-seat.unavailable {
    background-color: #D00000;
    cursor: not-allowed;
  }
  div.seatCharts-container {
    border-right: 1px solid #adadad;
    width: 54%;
    padding: 0 20px 0 0;
    float: left;
  }
  div.seatCharts-legend {
    padding-left: 0px;
  }
  ul.seatCharts-legendList {
    padding-left: 0px;
  }
  .seatCharts-legendItem{
    margin-top: 10px;
    line-height: 2;
  }
  span.seatCharts-legendDescription {
    margin-left: 5px;
    line-height: 30px;
  }
  .checkout-button {
    display: block;
    margin: 16px 0 22px;
    border:none;
    font-size: 16px;
    cursor: pointer;
    background: #D88A04;
    padding: 7px 11px;
    color: #fff;
    outline:none;
    transition: 0.5s all;
    -webkit-transition: 0.5s all;
    -o-transition: 0.5s all;
    -moz-transition: 0.5s all;
    -ms-transition: 0.5s all;
  }
  .checkout-button.book{
    float: left;
    margin-right: 10px;
  }
  .checkout-button:hover {
    background: #000;
    transition: 0.5s all;
    -webkit-transition: 0.5s all;
    -o-transition: 0.5s all;
    -moz-transition: 0.5s all;
    -ms-transition: 0.5s all;
  }
  #selected-seats {
    max-height: 84px;
    overflow-y: auto;
    overflow-x: none;
    width: 200px;
    }
  #selected-seats li{
      border: 1px solid #d3d3d3;
      background: #f7f7f7;
      margin: 6px 0;
      font-size: 14px;
      font-weight: bold;
      text-align: center;
      color: #000;
      font-weight: 600;
      padding: 6px 11px;
      width: 60%;
    }
  .scrollbar {
      overflow-y: scroll;
  }
  .booking-details p i {
      color: #000;
      font-size: 18px;
      line-height: 2.2em;
    font-weight:normal;
  }
  ul.book-left {
      float: left;
      width: 35%;
  }
  ul.book-right {
      float: right;
      width: 63%;
  }
  ul.book-left li {
      font-size: 16px;
      font-weight: 600;
      color: #D88A04;
      line-height: 1.9em;
  }
  ul.book-right li {
      font-size: 16px;
      font-weight: 500;
      color: #000;
      line-height: 1.9em;
  }
  
  /*-- responsive media queries --*/
  
  @media (max-width: 1440px){
    .main {
      width: 50%;
    }
    .content h1 {
      font-size: 33px;
    } 
  }
  @media (max-width: 1366px){
    .main {
      width: 53%;
    } 
  }
  @media (max-width: 1280px){
    .main {
      width: 56%;
    } 
  }
  @media (max-width: 1080px){
    .main {
      width: 67%;
    } 
  }
  @media (max-width: 1024px){
    div.seatCharts-container {
      width: 57%;
      padding: 0 13px 0 0;
    }
    .booking-details {
      width: 36%;
    } 
  }
  @media (max-width: 991px){
    div.seatCharts-container {
      width: 58%;
    }
    .content h1 {
      font-size: 31px;
    } 
  }
  @media (max-width: 800px){
    .main {
      width: 79%;
    }
    /*-- w3layouts --*/
    .booking-details {
      width: 33%;
    }
    div.seatCharts-container {
      width: 62%;
    }
    .front {
      margin: 5px 9px 45px 38px;
    }
    ul.book-right {
      width: 57%;
    }
    .main h2 {
      font-size: 25px;
    } 
  }
  @media (max-width: 768px){
    div.seatCharts-container {
      width: 60%;
    }
    .main {
      width: 85%;
    }
    .content {
      padding: 112px 0;
    } 
  }
  @media (max-width: 736px){
    div.seatCharts-container {
      width: 62%;
    }
    .content h1 {
      font-size: 27px;
    }
    .main h2 {
      font-size: 24px;
    }
    .content {
      padding: 74px 0;
    } 
  }
  @media (max-width: 667px){
    div.seatCharts-container {
      width: 100%;
      padding: 0 0px 0 0;
        border: none;
    } 
    .booking-details {
      width: 100%;
      margin-top: 35px;
    }
    /*-- agileits --*/
    .main {
      width: 59%;
    }
    .content h1 {
      font-size: 24px;
    }
  }
  @media (max-width: 640px){
    .content {
      padding: 59px 0;
    }
    .main {
      width: 61%;
    }
    .front {
      margin: 5px 6px 45px 6px;
    } 
  }
  @media (max-width: 600px){
    .main h2 {
      font-size: 21px;
    }
    .main {
      width: 65%;
    }
    .front {
      margin: 5px 6px 36px 6px;
    } 
  }
  @media (max-width: 568px){
    .main {
      width: 68%;
    } 
  }
  @media (max-width: 480px){
    .main {
      padding: 18px 18px;
      width: 81%;
    }
    p.copy_rights {
      font-size: 13px;
      line-height: 1.8em;
      width: 90%;
      margin: 0 auto;
    } 
  }
  @media (max-width: 414px){
    .main {
      width: 85%;
    }
    div.seatCharts-cell {
      height: 25px;
      width: 25px;
      line-height: 26px;
      margin: 3px;
      font-size: 12px;
    } 
    .content h1 {
      font-size: 22px;
    }
    .main h2 {
      margin-bottom: 21px;
    }
  }
  @media (max-width: 384px){
    div.seatCharts-cell {
      height: 23px;
      width: 23px;
      line-height: 24px;
    } 
    .content {
      padding: 45px 0;
    }
  }
  @media (max-width: 375px){
    .main h2 {
      font-size: 20px;
    }
    div.seatCharts-cell {
      margin: 2px 3px 2px 2px;
    }
    .main {
      margin: 28px auto;
    }
    ul.book-right li ,ul.book-left li {
      font-size: 14px;
    }
    .content h1 {
      font-size: 19px;
    }
  }
  @media (max-width: 320px){
    .content {
      padding: 41px 0;
    }
    .main h2 {
      font-size: 17px;
    }
    .front {
      margin: 5px 6px 25px 6px;
      font-size: 14px;
      padding: 7px 0;
    }
    .content h1 {
      font-size: 18px;
    }
    div.seatCharts-cell {
      margin: 2px 2px 2px 2px;
    }
    .main {
      padding: 15px 8px;
      width: 90%;
    }
    div.seatCharts-cell {
      height: 22px;
      width: 22px;
      line-height: 22px;
      font-size: 11px;
    }
    .checkout-button {
      font-size: 14px;
    }
    span.seatCharts-legendDescription {
      line-height: 0px;
      font-size: 13px;
    }
  }
  </style>
  
  