<script lang="ts">
interface GameData {
  choices: string[];
  userChoice: string;
  computerChoice: string;
  result: string;
  userScore: number;
  computerScore: number;
}

export default {
  data(): GameData {
    return {
      choices: ['rock', 'paper', 'scissors', 'lizard', 'spock'],
      userChoice: '',
      computerChoice: '',
      result: '',
      userScore: 0,
      computerScore: 0
    }
  },
  methods: {
    playGame(): void {
      this.computerChoice = this.choices[Math.floor(Math.random() * this.choices.length)];
      // Logic for Rock, Paper, Scissors, Lizard, Spock
      if (this.userChoice === this.computerChoice) {
        this.result = 'It\'s a tie!'
      } else if (
        (this.userChoice === 'rock' && (this.computerChoice === 'scissors' || this.computerChoice === 'lizard')) ||
        (this.userChoice === 'paper' && (this.computerChoice === 'rock' || this.computerChoice === 'spock')) ||
        (this.userChoice === 'scissors' && (this.computerChoice === 'paper' || this.computerChoice === 'lizard')) ||
        (this.userChoice === 'lizard' && (this.computerChoice === 'spock' || this.computerChoice === 'paper')) ||
        (this.userChoice === 'spock' && (this.computerChoice === 'rock' || this.computerChoice === 'scissors'))
      ) {
        this.result = 'You win!'
        this.userScore++
      } else {
        this.result = 'You lose!'
        this.computerScore++
      }
    },
    resetGame(): void {
      this.userChoice = ''
      this.computerChoice = ''
      this.result = ''
      this.userScore = 0
      this.computerScore = 0
    },
    capitalizeFirstLetter(word: string): string {
      return word.charAt(0).toUpperCase() + word.slice(1);
    },
  },
  mounted(): void {
    // Fetch saved scores from local storage for the bonus feature
  }
}

</script>

<template>
  <header>
    <div class="wrapper">
      <div>
        <h1>Rock, Paper, Scissors</h1>
        <div>
          <button v-for="choice in choices" :key="choice" @click="userChoice = choice">{{ capitalizeFirstLetter(choice) }}</button>

        </div>
        <div>
          <p v-if="userChoice">Your Choice: {{ capitalizeFirstLetter(userChoice) }}</p>
          <p v-if="computerChoice">Computer's Choice: {{ capitalizeFirstLetter(computerChoice) }}</p>
          <p v-if="result">{{ result }}</p>
        </div>
        <div>
          <p>User Score: {{ userScore }}</p>
          <p>Computer Score: {{ computerScore }}</p>
        </div>
        <div>
          <button @click="playGame">Play</button>
          <button @click="resetGame">Reset</button>
        </div>
      </div>
    </div>
  </header>


</template>