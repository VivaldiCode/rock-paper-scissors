<script lang="ts">
import HeaderComponent from '@/components/HeaderComponent.vue'

interface GameData {
  choices: string[];
  userChoice: string;
  computerChoice: string;
  result: string;
  resultClass: string;
  userScore: number;
  computerScore: number;
  isActive: boolean;
  rulesActive: boolean;
}

export default {
  components: { HeaderComponent },
  data(): GameData {
    return {
      choices: ['rock', 'paper', 'scissors', 'lizard', 'spock'],
      userChoice: '',
      computerChoice: '',
      result: '',
      resultClass: '',
      userScore: 0,
      computerScore: 0,
      isActive: false,
      rulesActive: false
    }
  },

  methods: {
    playGame(): void {
      this.computerChoice = this.choices[Math.floor(Math.random() * this.choices.length)]
      // Logic for Rock, Paper, Scissors, Lizard, Spock
      if (this.userChoice === this.computerChoice) {
        this.result = 'It\'s a tie!'
        this.resultClass = 'tie'
      } else if (
        (this.userChoice === 'rock' && (this.computerChoice === 'scissors' || this.computerChoice === 'lizard')) ||
        (this.userChoice === 'paper' && (this.computerChoice === 'rock' || this.computerChoice === 'spock')) ||
        (this.userChoice === 'scissors' && (this.computerChoice === 'paper' || this.computerChoice === 'lizard')) ||
        (this.userChoice === 'lizard' && (this.computerChoice === 'spock' || this.computerChoice === 'paper')) ||
        (this.userChoice === 'spock' && (this.computerChoice === 'rock' || this.computerChoice === 'scissors'))
      ) {
        this.result = 'You win!'
        this.resultClass = 'win'

        this.userScore++
      } else {
        this.result = 'You lose!'
        this.resultClass = 'lose'

        if (this.userScore > 0) this.userScore--
      }
    },
    makeUserChoice(choice: string): void {
      this.userChoice = choice
      this.isActive = true
      document.body.classList.add('modal-active')
      this.playGame()
    },
    resetGame(): void {
      this.userChoice = ''
      this.computerChoice = ''
      this.result = ''
      this.resultClass = ''
      this.isActive = false
      document.body.classList.remove('modal-active')


    },
    capitalizeFirstLetter(word: string): string {
      return word.charAt(0).toUpperCase() + word.slice(1)
    }
  },
  mounted(): void {
    // Fetch saved scores from local storage for the bonus feature
  }
}

</script>

<template>
  <div class="wrapper">
    <div>
      <HeaderComponent :userScore="userScore" :computerScore="computerScore" />
      <div class="choice choice-l1">
        <button class="scissors" @click="makeUserChoice('scissors')"></button>
      </div>
      <div class="choice choice-l2">
        <button class="spock" @click="makeUserChoice('spock')"></button>
        <button class="paper" @click="makeUserChoice('paper')"></button>
      </div>
      <div class="choice choice-l3">
        <button class="lizard" @click="makeUserChoice('lizard')"></button>
        <button class="rock" @click="makeUserChoice('rock')"></button>
      </div>
    </div>
    <button class="rules-button" @click="rulesActive=true">RULES</button>
  </div>
  <div id="modal-container" :class="{ active: isActive, four: isActive }">
    <div class="modal-background">
      <div class="modal">
        <div class="result">
          <div class="result-button">
            <h1 v-if="userChoice">You Pick:</h1>
            <div class="choice choice-result">
              <button
                :class="userChoice + ' '  + ((resultClass === 'win' || resultClass === 'tie') ? 'wave' : '')"></button>
            </div>
          </div>
          <div class="result-status">
            <h1 v-if="result">{{ result }}</h1>
            <button @click="resetGame">Play Again</button>
          </div>
          <div class="result-button">
            <h1 v-if="computerChoice">House Pick:</h1>
            <div class="choice choice-result">
              <button
                :class="computerChoice + ' ' + ((resultClass === 'lose' || resultClass === 'tie') ? 'wave' : '')"></button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div id="modal-container" :class="{ active: rulesActive, four: rulesActive }">
    <div class="modal-background">
      <div class="modal">
        <div @click="rulesActive=false" class="close-button">x</div>
        <div>
          <img src="@/assets/image-rules-bonus.svg" alt="rules">
        </div>
      </div>
    </div>
  </div>
</template>