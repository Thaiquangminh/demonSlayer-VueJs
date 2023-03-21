<template>
  <header>
    <h1>Monster Slayer</h1>
  </header>
  <div id="game">
    <section id="monster" class="container">
      <h2>Monster Health</h2>
      <div class="healthbar">
        <div class="healthbar__value" :style="{width: healthMonsterValue}"></div>
      </div>
    </section>
    <section id="player" class="container">
      <h2>Your Health</h2>
      <div class="healthbar">
        <div class="healthbar__value" :style="{width: healthPlayerValue}"></div>
      </div>
    </section>
    <section class="container" v-if="winner !== ''">
      <h2>Game Over!</h2>
      <h3 v-if="winner === 'monster'">You lost</h3>
      <h3 v-if="winner === 'player'">You win</h3>
      <h3 v-if="winner === 'draw'">Draw</h3>
      <button @click="resetGame()">PLAY AGAIN</button>
    </section>
    <section id="controls" v-else-if="winner === ''">
      <button @click="attackMonster()">ATTACK</button>
      <button :disabled="count < 3" @click="specialAttack()">SPECIAL ATTACK</button>
      <button @click="healPlayer()">HEAL</button>
      <button @click="surrender()">SURRENDER</button>
    </section>
    <section id="log" class="container">
      <h2>Battle Log</h2>
      <ul></ul>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      count: 0,
      winner: ''
    }
  },
  computed: {
    healthMonsterValue() {
      return this.monsterHealth + '%'
    },
    healthPlayerValue() {
      return this.playerHealth + '%'
    }

  },
  watch: {
    playerHealth(newValue) {
      if(newValue <= 0 && this.monsterHealth <= 0) {
        this.winner = 'draw'
      }
      else if(newValue <= 0) {
        this.winner = 'monster'
      }
    },
    monsterHealth(newValue) {
      if(newValue <= 0 && this.playerHealth <= 0) {
        this.winner = 'draw'
      }
      else if(newValue <= 0) {
        this.winner = 'player'
      }
    }
  },
  methods: {
    getRandomValue(min, max) {
      return Math.floor(Math.random()*(max-min) + min)
    },
    attackMonster() {
      this.count += 1
      const attackValue =  this.getRandomValue(5, 12)
      this.monsterHealth -= attackValue
      this.attackPlayer()
    },
    attackPlayer() {
      const attackValue =  this.getRandomValue(8, 12)
      this.playerHealth -= attackValue
    },
    specialAttack() {
      this.count -= 3
      const attackValue = this.getRandomValue(10, 20)
      this.monsterHealth -= attackValue
      this.attackPlayer()
    },
    healPlayer() {
      const healValue = this.getRandomValue(5,18)
      if(this.playerHealth + healValue > 100) {
        this.playerHealth = 100
      }
      else {
        this.playerHealth += healValue
      }
    },
    resetGame() {
      this.playerHealth = 100
      this.monsterHealth = 100
      this.count = 0
      this.winner = ''
    },
    surrender() {
      this.winner = 'monster'
    }
  }

}
</script>

<style scoped>
* {
  box-sizing: border-box;
}

html {
  font-family: 'Jost', sans-serif;
}

body {
  margin: 0;
}

header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 0.5rem;
  background-color: #880017;
  color: white;
  text-align: center;
  margin-bottom: 2rem;
}

section {
  width: 90%;
  max-width: 40rem;
  margin: auto;
}

.healthbar {
  width: 100%;
  height: 40px;
  border: 1px solid #575757;
  margin: 1rem 0;
  background: #fde5e5;
}

.healthbar__value {
  background-color: #00a876;
  width: 100%;
  height: 100%;
}

.container {
  text-align: center;
  padding: 0.5rem;
  margin: 1rem auto;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 12px;
}

#monster h2,
#player h2 {
  margin: 0.25rem;
}

#controls {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}

button {
  font: inherit;
  border: 1px solid #88005b;
  background-color: #88005b;
  color: white;
  padding: 1rem 2rem;
  border-radius: 12px;
  margin: 1rem;
  width: 12rem;
  cursor: pointer;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}

button:focus {
  outline: none;
}

button:hover,
button:active {
  background-color: #af0a78;
  border-color: #af0a78;
  box-shadow: 1px 1px 8px rgba(0, 0, 0, 0.26);
}

button:disabled {
  background-color: #ccc;
  border-color: #ccc;
  box-shadow: none;
  color: #3f3f3f;
  cursor: not-allowed;
}

#log ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

#log li {
  margin: 0.5rem 0;
}

.log--player {
  color: #7700ff;
}

.log--monster {
  color: #da8d00;
}

.log--damage {
  color: red;
}

.log--heal {
  color: green;
}
</style>