I began by examining the existing studentsData and identifying the output structure I wanted to achieve. The main objective was to display each student in a responsive, styled grid while supporting sorting and filtering features for a better user experience.

The first step was to transform the raw studentsData into a more usable format, calculating derived values such as age and average score. This transformation was handled in a single map() call so that the rest of the app could work with clean, consistent data.

Next, I implemented dynamic sorting and filtering logic using Svelte’s reactive $: syntax. This ensured the displayed students updated automatically when the sort mode or filter toggles changed.

For layout, I used CSS Grid with auto-fit and minmax() for flexibility, and added a media query to ensure that on smaller screens the layout collapses into a single column. Each .box is styled with a consistent aspect ratio and subtle hover effects for a polished UI.

Assumptions Made:

- Passing Score Threshold: Any average score above 60% is considered a passing score.
- UI Placement: The most intuitive place for sorting and filtering controls is in the header, positioned beside the Students title.
- Data Completeness: All students have valid birthdate and scores data (no missing fields).

Additional Features Implemented:

Sorting:
    -By Name (ascending, A–Z)
    -By Score (descending, highest first)

Filtering:
    -"Show Active Only" toggle to hide inactive students.

