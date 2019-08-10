<script>
export default {
  data: {
    player: [
      {
        lvl: 1,
        currentHP: 100,
        maxHP: 100,
        attack: 10,
        dblAtk: 10,
        dblAtkLeft: 3,
        specialAtk: 25,
        spcAtkLeft: 1,
        hpPot: 3
      },
      {
        lvl: 2,
        currentHP: 200,
        maxHP: 200,
        attack: 25,
        dblAtk: 15,
        dblAtkLeft: 3,
        specialAtk: 50,
        spcAtkLeft: 1,
        hpPot: 4
      },
      {
        lvl: 3,
        currentHP: 500,
        maxHP: 500,
        dblAtk: 25,
        dblAtkLeft: 3,
        specialAtk: 70,
        spcAtkLeft: 1,
        hpPot: 5
      },
      {
        lvl: 4,
        currentHP: 1000,
        maxHP: 1000,
        dblAtk: 40,
        dblAtkLeft: 5,
        specialAtk: 200,
        spcAtkLeft: 1,
        hpPot: 10
      },
      {
        lvl: 5,
        currentHP: 2500,
        maxHP: 2500,
        dblAtk: 150,
        dblAtkLeft: 5,
        specialAtk: 500,
        spcAtkLeft: 1,
        hpPot: 20
      },
      {
        lvl: 6,
        currentHP: 5000,
        maxHP: 5000,
        dblAtk: 300,
        dblAtkLeft: 5,
        specialAtk: 800,
        spcAtkLeft: 1,
        hpPot: 20
      }
    ],
    monsters: [
      {
        lvl: 1,
        name: "Monster ONE",
        currentHP: 100,
        maxHP: 100,
        attack: 20
      },
      {
        lvl: 2,
        name: "Monster Two",
        currentHP: 200,
        maxHP: 200,
        attack: 40
      },
      {
        lvl: 3,
        name: "Monster three",
        currentHP: 500,
        maxHP: 500
      },
      {
        lvl: 4,
        name: "Monster four",
        currentHP: 1000,
        maxHP: 1000
      },
      {
        lvl: 5,
        name: "Monster five",
        currentHP: 2500,
        maxHP: 2500
      },
      {
        lvl: 6,
        name: "Monster six",
        currentHP: 5000,
        maxHP: 5000
      }
    ]
  }
};
</script>
