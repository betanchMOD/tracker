<script setup lang="ts">
defineProps<{
	currentStep: string;
	steps: string[];
	size?: 'sm' | 'lg'; // sm = home page mini stepper, lg = full project page
}>();

const getMilestoneIndex = (step: string, steps: string[]) => steps.indexOf(step);

const milestoneIcons: Record<string, string> = {
	Discovery: 'i-heroicons-magnifying-glass',
	Design: 'i-heroicons-pencil-square',
	Development: 'i-heroicons-cog-6-tooth',
	Testing: 'i-heroicons-beaker',
	Launch: 'i-heroicons-rocket-launch',
};
</script>

<template>
	<ul :class="['flex w-full', size === 'lg' ? 'justify-center gap-6 items-center' : 'items-center gap-0']">
		<template v-for="(step, i) in steps" :key="step">
			<li class="flex flex-col items-center gap-1 text-center relative">
				<!-- Large View -->
				<div v-if="size === 'lg'" class="flex flex-col items-center gap-1">
					<div :class="[
						'relative flex items-center justify-center rounded-full flex-shrink-0 h-10 w-10 border-2 transition-all duration-500',
						step === currentStep
							? step === steps.at(-1)
								? 'bg-primary text-white animate-pulse ring-2 ring-primary/40 ring-offset-2'
								: 'animate-pulse border-primary text-primary ring-2 ring-primary/40 ring-offset-2'
							: '',
						steps.indexOf(step) < steps.indexOf(currentStep)
							? 'bg-primary text-white'
							: step === currentStep
								? ''
								: 'dark:border-gray-700 border-gray-300 text-gray-400',
					]">
						<UIcon :name="milestoneIcons[step]" class="w-5 h-5" />
					</div>
					<p class="text-sm text-gray-600 dark:text-gray-400">{{ step }}</p>
				</div>

				<!-- Small View -->
				<div v-else class="flex flex-col items-center">
					<div :class="[
						'flex items-center justify-center w-9 h-9 rounded-full transition-all duration-500',
						step === currentStep
							? 'animate-pulse border-2 border-primary text-primary ring-2 ring-primary/40 ring-offset-2'
							: '',
						i <= getMilestoneIndex(currentStep, steps)
							? 'bg-primary text-white'
							: 'border border-gray-400 text-gray-400',
					]">
						<UIcon :name="milestoneIcons[step]" class="w-4 h-4" />
					</div>
					<!-- Adjusted spacing and alignment -->
					<span class="text-xs text-gray-500 dark:text-gray-400 mt-1 leading-none text-center w-20">
						{{ step }}
					</span>
				</div>

			</li>

			<!-- Divider -->
			<li v-if="i !== steps.length - 1" class="flex items-center justify-center flex-shrink-0"
				:class="size === 'lg' ? 'w-6' : 'w-4'" aria-hidden="true">
				<div class="h-0.5 w-full"
					:class="[i < getMilestoneIndex(currentStep, steps) ? 'bg-primary' : 'bg-gray-300 dark:bg-gray-700']" />
			</li>
		</template>
	</ul>
</template>
