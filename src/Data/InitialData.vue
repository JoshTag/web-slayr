<script>
export default {
  data: {
    player: [
      {
        lvl: 1,
        currentHP: 100,
        maxHP: 100,
        attack: 10,
        dblAtk: 8,
        dblAtkLeft: 3,
        specialAtk: 25,
        spcAtkLeft: 1,
        hpPot: 3
      }
    ],
    monsters: [
      {
        lvl: 1,
        name: "Monster ONE",
        currentHP: 100,
        maxHP: 100,
        attack: 15
      },
      {
        lvl: 2,
        name: "Monster Two",
        currentHP: 120,
        maxHP: 120,
        attack: 20
      },
      {
        lvl: 3,
        name: "Salty P",
        currentHP: 150,
        maxHP: 150,
        attack: 30
      }
    ]
  }
};
</script>
