<script lang="ts">
  import { studentsData } from './lib/data';

  type Student = {
    id: string;
    name: string;
    age: number;
    averageScore: number;
    activeLabel: 'Yes' | 'No';
  };

  let sortMode: 'none' | 'name' | 'score' = 'none';
  let showActiveOnly = false;

  let updatedStudents: Student[] = studentsData.map((stu) => {
    const birthDate = new Date(stu.birthdate);
    const age = new Date().getFullYear() - birthDate.getFullYear();
    const averageScore =
      (stu.scores.math + stu.scores.english + stu.scores.science) / 3;
    return {
      id: String(stu.id),
      name: `${stu.firstName} ${stu.lastName}`,
      age,
      averageScore,
      activeLabel: stu.isActive ? 'Yes' : 'No'
    };
  });

  $: displayedStudents = updatedStudents
    .filter((s) => (showActiveOnly ? s.activeLabel === 'Yes' : true))
    .sort((a, b) => {
      if (sortMode === 'name') return a.name.localeCompare(b.name);
      if (sortMode === 'score') {
        if (a.averageScore > b.averageScore) {
          return -1;
        } else if (a.averageScore < b.averageScore) {
          return 1;
        } else {
          return 0;
        }
      }
      return 0;
    });
</script>
  <header>
    <div class="header">
      <h1> Students </h1>
      <div class="sort-buttons">
        <button class:selected={sortMode === 'name'} on:click={() => sortMode = 'name'}>
          Sort by Name ▲
        </button>
        <button class:selected={sortMode === 'score'} on:click={() => sortMode = 'score'}>
          Sort by Score ▼
        </button>
        <button class:selected={sortMode === 'none'} on:click={() => sortMode = 'none'}>
          Clear Sort
        </button>
        <label>
          <input type="checkbox" bind:checked={showActiveOnly} />
          Show Active Only
        </label>
      </div>
    </div>
  </header>
  <main>
  {#each displayedStudents as student}
    <div class="box">
      <h2>{student.name}</h2>
      <div class="stat">
        <span class="stat-row">Age:</span>
        <span class="stat-val">{student.age}</span>
      </div>
      <div class="stat">
        <span class="stat-row">Average score: </span>
        <span class="stat-val">{Math.round(student.averageScore)}</span>
      </div>
      <div class="stat">
        <span class="stat-row">Active: </span>
        <span class="stat-val">{student.activeLabel}</span>
      </div>
      <div class="stat">
        <span class="stat-row">Passed:</span>
        <span class="stat-val">{student.averageScore > 60 ? 'Yes' : 'No'}</span>
      </div>
      <div class="stat">
        <span class="stat-row">ID: </span>
        <span class="stat-val">{student.id}</span>
      </div>
    </div>
  {/each}
</main>

<style>
  :global(body) {
    background: #F7F3ED;
    color: #4B3D47;
    margin: 0;
    font-family: Tahoma;
    font-weight: 700;
    line-height: 100%;
    letter-spacing: -2%;
  }

  main {
    max-width: 1100px;
    margin: 0 auto;
    padding: 2rem;
  }

  .header {
    max-width: 1100px;
    margin: 0 auto;
    padding: 4rem 2rem 0rem 3rem;
    font-family: 'Tahoma';
    display: flex;
    flex-wrap: wrap;
    gap: 2rem;
  }

  .box {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 20px;
    background: white;
    border-radius: 8px;
    aspect-ratio: 3/2;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2); 
}

  .stat {
    display: flex;
    justify-content: space-between;
  }
  .stat-row {
    font-weight:lighter;
  }

  .stat-val {
    font-weight: bold;
  }

  .box:hover {
    transform: translateY(-3px);
    box-shadow: 0 8px 18px rgba(0,0,0,0.08);
  }

  h2 {
    margin: 0 0 0.5rem 0;
  }

  :global(main) {
    display: grid;
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  @media (min-width: 700px) {
    :global(main) {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  @media (min-width: 1000px) {
    :global(main) {
      grid-template-columns: repeat(3, 1fr);
    }
  }

</style>
