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
        currentHP: 150,
        maxHP: 150,
        attack: 15,
        dblAtk: 12,
        dblAtkLeft: 3,
        specialAtk: 30,
        spcAtkLeft: 1,
        hpPot: 4
      },
      {
        lvl: 3,
        currentHP: 200,
        maxHP: 200,
        dblAtk: 15,
        dblAtkLeft: 3,
        specialAtk: 35,
        spcAtkLeft: 1,
        hpPot: 5
      },
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
        currentHP: 150,
        maxHP: 150,
        attack: 30
      },
{
        lvl: 3,
        name: "Monster three",
        currentHP: 200,
        maxHP: 200
      }
    ]
  }
};
</script>
