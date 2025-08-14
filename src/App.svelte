<script lang="ts">
  import { studentsData } from './lib/data';
  import 'emoji-picker-element';

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

<header class="dashboard-header">
  <h1>📊 Student Dashboard</h1>
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
</header>

<main>
  {#each displayedStudents as student}
    <div class="box">
      <h2>{student.name}</h2>

      <p class="age">
        {#if student.age > 25}
          Mature Student — {student.age} years old
        {:else}
          Young Student — {student.age} years old
        {/if}
      </p>

      <div class="score"> 🎯 Avg score: {Math.round(student.averageScore)}</div>
      <div class:is-red={student.activeLabel === 'No'} class:is-green={student.activeLabel === 'Yes'}>
        {student.activeLabel === 'Yes' ? '✅️ Active' : '❌️ Inactive'}
      </div>
    </div>
  {/each}
</main>

<style>
  :global(body) {
    font-family: 'Segoe UI';
    background: lightblue;
    color: #333;
    margin: 0;
  }

  main {
    max-width: 1100px;
    margin: 0 auto;
    padding: 2rem;
  }

  h1 {
    padding: 2rem;
    display: flex;
    justify-content: center;
    align-content: center;
  }

  .sort-buttons {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    justify-content: center;
    align-items: center;
    margin-bottom: 2rem;
  }

  .sort-buttons button {
    padding: 0.5rem 1rem;
    border: none;
    background: #f0f4f8;
    border-radius: 6px;
    cursor: pointer;
    transition: background 0.5s;
  }

  .sort-buttons button:hover {
    background: #e3eaf1;
  }

  .sort-buttons button.selected {
    background: #4a90e2;
    color: white;
  }

  @media (max-width: 600px) {
    .sort-buttons {
      flex-direction: column;
      align-items: stretch;
    }
  }

  :global(main) {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
    gap: 1.5rem;
  }

  .box {
    background: white;
    display: block;
    border-radius: 12px;
    padding: 1.5rem;
    box-shadow: 0 4px 10px rgba(0,0,0,0.05);
    border-top: 5px solid #4a90e2;
  }

  .box:hover {
    transform: translateY(-3px);
    box-shadow: 0 8px 18px rgba(0,0,0,0.08);
  }

  h2 {
    margin: 0 0 0.5rem 0;
    color: #4a90e2;
    font-size: 1.2rem;
  }

  .age {
    font-size: 0.95rem;
    margin-bottom: 0.75rem;
    color: #666;
  }

  .score {
    font-weight: 600;
    margin-bottom: 0.5rem;
    color: #333;
  }

  .is-green {
    color: #28a745;
    font-weight: bold;
  }

  .is-red {
    color: #d9534f;
    font-weight: bold;
  }
</style>
