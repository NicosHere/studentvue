<script context="module">
	export async function load({ session }) {
		if (!session.user) {
			return {
				status: 302,
				redirect: '/login'
			}
		}
		return {}
	}
</script>

<script>
	import { session } from '$app/stores'
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>

<div class="layout">
	<h1>Hello, {$session.student.FormattedName.split(' ')[0]}!</h1>
	<div class="average value box">
		<h1 style={$session.gradebook.averageStyle}>
			{$session.gradebook.average}
		</h1>
		<div class="value-label">Average grade<br />&nbsp;</div>
	</div>
	<div class="improvement value box">
		<h1 style={$session.gradebook.week.averageStyle}>
			{$session.gradebook.week.average}
		</h1>
		<div class="value-label">Average grade<br />this week</div>
	</div>
	<div class="week-assignments value box">
		<h1>{$session.gradebook.week.length}</h1>
		<div class="value-label">
			{$session.gradebook.week.length === 1 ? 'Assignment' : 'Assignments'}
			<br />this week
		</div>
	</div>
	<div class="days value box">
		<h1>
			{$session.gradebook.days}
		</h1>
		<div class="value-label">
			{$session.days === 1 ? 'Day' : 'Days'} left in
			<br />{$session.gradebook.ReportingPeriod.GradePeriod}
		</div>
	</div>
	<div class="grades box">
		<a class="link" sveltekit:prefetch href="/grades"><h2>Grades</h2></a>
		<table class="grades-table">
			{#each $session.gradebook.Courses.Course as course, index}
				<tr>
					<td class="course-name">
						<a class="link" sveltekit:prefetch href={'/course/' + index}>
							{course.Title}
						</a>
					</td>
					<td class="course-grade" style={course.style}>
						{course.Marks.Mark.CalculatedScoreString}
					</td>
					<td class="course-score" style={course.style}>{course.score}</td>
				</tr>
			{/each}
		</table>
	</div>
	<div class="assignments box">
		<div class="assignments-scroll">
			<a class="link" sveltekit:prefetch href="/assignments"><h2>Assignments</h2></a>
			<table class="assignments-table">
				{#each $session.gradebook.assignments as assignment}
					{#if assignment.scorePercent >= 0}
						<tr>
							<td class="assignment-name">{assignment.Measure}</td>
							<td class="assignment-score" style={assignment.style}>
								{assignment.score}
							</td>
						</tr>
					{/if}
				{/each}
			</table>
		</div>
	</div>
</div>

<style>
	.layout {
		display: grid;
		height: 100%;
		width: 100%;
		gap: var(--spacing);
		grid-template-columns: 1fr 1fr 1fr 1fr 2fr;
		grid-template-rows: auto auto 1fr;
	}

	h1 {
		margin-bottom: 0px;
		grid-column: 1 / 5;
	}

	.value {
		aspect-ratio: 1;
		text-align: center;
		display: flex;
		flex-direction: column;
		min-width: 0;
		min-height: 0;
		overflow: hidden;
		font-size: 0.9em;
		padding: 0;
	}
	.value h1 {
		margin-top: auto;
		margin-bottom: 10px;
	}
	.value div {
		margin-bottom: auto;
	}

	.grades {
		grid-column: 1 / 5;
	}

	.assignments {
		padding: 0;
		grid-column: 5;
		grid-row: 1 / 4;
		overflow: hidden;
	}

	.assignments-scroll {
		height: 100%;
		overflow-y: auto;
		scrollbar-color: var(--bg-color-2-5) transparent;
		padding: var(--spacing);
	}

	.grades-table {
		height: calc(100% - 2 * var(--spacing));
	}

	.assignments-table {
		table-layout: fixed;
		width: 100%;
	}

	.assignments-table td {
		padding-top: 10px;
		padding-bottom: 10px;
	}

	.assignment-name {
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
		width: 100%;
	}

	.assignment-score {
		width: 80px;
		text-align: right;
		white-space: nowrap;
		padding-left: 10px;
	}

	.course-grade {
		padding: 0;
		font-weight: bold;
	}

	.course-score {
		text-align: right;
	}
</style>
