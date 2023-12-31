<script setup lang="ts">
import { socket } from '@/socket';
import AchievementDisplay from './Achievement.vue';
import { type Achievement, type UserInfo } from '@/types';
import { computed, onUnmounted, ref } from 'vue';

const props = defineProps<{
    userProp: UserInfo
}>()

const emit = defineEmits(['toggleFriendDisplay']);

const displayAll = ref(false);

function toggleAchDisplay() {
    displayAll.value = !displayAll.value
}

function toggleFriends() {
    emit('toggleFriendDisplay');
}

const progressAchToDisplay = computed(() => {
    let i:number = 0;
    for (let ach of gameProgressAchievements) {
        if (ach.progress() != 1 || i == gameProgressAchievements.length - 1)
            return (i);
        i++;
    }
    return (i);
})

/* In normal mode: display only first value that is not = 1 or last value if all = 1 */
const gameProgressAchievements: Achievement[] = [
	{
        name: "Getting Started",
        imageUrl: "/assets/Achievements/1game.png",
        description: "Play one game.",
        progress: () => {
            const val = (props.userProp.win * 1 + props.userProp.loss * 1) / 1
            return val < 1 ? val : 1; /* Return only val in order to overflow 1 and not to be displayed in list */
        },
		current: (props.userProp.win * 1 + props.userProp.loss * 1 === 0) ? 0 : -1,
        max: 1,
    },
    {
        name: "Lifeguard",
        imageUrl: "/assets/Achievements/19.svg",
        description: "Play 19 games",
        progress: () => {
            const val = (props.userProp.win * 1 + props.userProp.loss * 1) / 19;
            return val < 1 ? val : 1;
        },
		current: (props.userProp.win * 1 + props.userProp.loss * 1 < 19) ? +props.userProp.win + (+props.userProp.loss) : -1,
        max: 19,
    },
    {
        name: "Welcome to the Jar",
        imageUrl: "/assets/Achievements/42_logo.svg",
        description: "Play 42 games",
        progress: () => {
            const val = (props.userProp.win * 1 + props.userProp.loss * 1) / 42;
            return val < 1 ? val : 1;
        },
		current: props.userProp.win * 1 + props.userProp.loss * 1,
        max: 42,
    },   
]

const achievementList: Achievement[] = [   
    {
        name: "Master",
        imageUrl: "/assets/Achievements/100games.png",
        description: "Win 50 games",
        progress: () => {
            const val = (props.userProp.win * 1) / 50
            return val < 1 ? val : 1;
        },
		current: props.userProp.win * 1,
        max: 50,  
    },
    {
        name: "Incognito",
        imageUrl: "/assets/Achievements/incognito.png",
        description: "Change username",
        progress: () => {
			 return props.userProp.achievements & 1 ? 1 : 0;
		},
        current: -1,
		max: 1,
    },
    {
        name: "Make up Artist",
        imageUrl: "/assets/Achievements/makeup.png",
        description: "Change your profile picture",
        progress: () => {
			return props.userProp.achievements & 2 ? 1 : 0;
		},
		current: -1,
        max: 1,
    },
	{
		name: "Incognhugo",
		imageUrl: "/assets/Achievements/hugos.png",
		description: "One of us",
		progress: () => {
			return props.userProp.achievements & 256 ? 1 : 0;
		},
		current: -1,
		max: 1,
	},
    {
        name: "Flawless",
        imageUrl: "/assets/Achievements/flawless.png",
        description: "Win a game without conceding any points",
        progress: () => {
			return props.userProp.achievements & 4 ? 1 : 0;
		},
		current: -1,
        max: 1,
    },
    {
		name: "Is this multiplayer ?",
		imageUrl: "/assets/Achievements/touch5.png",
		description: "Hit the ball 5 times in a row without your opponent hitting it once",
		progress: () => {
			return props.userProp.achievements & 2048 ? 1 : 0;
		},
		current: -1,
		max: 1,
	},
	{
		name: "Telekinesis",
		imageUrl: "/assets/Achievements/telekinesis.png",
		description: "Win a game without moving",
		progress: () => {
			return props.userProp.achievements & 128 ? 1 : 0;
		},
		current: -1,
		max: 1,
	},
	{
		name: "Double The Trouble",
		imageUrl: "/assets/Achievements/finish20.png",
		description: "Finish a game with a score above 20",
		progress: () => {
			return props.userProp.achievements & 512 ? 1 : 0;
		},
		current: -1,
		max: 1,
	},
	{
		name: "All For Nothing",
		imageUrl: "/assets/Achievements/lose20.png",
		description: "Lose a game with a score above 20",
		progress: () => {
			return props.userProp.achievements & 1024 ? 1 : 0;
		},
		current: -1,
		max: 1,
	},
    {
        name: "You and Me",
        imageUrl: "/assets/Achievements/handshake.png",
        description: "Make your first friend",
        progress: () => {
			return props.userProp.achievements & 8 ? 1 : 0;
		},
		current: -1,
        max: 1,
    },
    {
        name: "This is a library",
        imageUrl: "/assets/Achievements/silence.png",
        description: "Did you know you could hide chat in game ?",
        progress: () => {
			return props.userProp.achievements & 4096 ? 1 : 0;
		},
		current: -1,
        max: 1,
    },
    {
        name: "Retro Gamer",
        imageUrl: "/assets/Achievements/retro.png",
        description: "Enter God Mode",
        progress: () => {
			return props.userProp.achievements & 16 ? 1 : 0;
		},
		current: -1,
        max: 1,
    },
	{
		name: "G.O.L.D.",
		imageUrl: "/assets/Achievements/gold.png",
		description: "Play with the golden ball",
		progress: () => {
			return props.userProp.achievements & 64 ? 1 : 0;
		},
		current: -1,
		max: 1,
	},
    {
        name: "Shielded",
        imageUrl: "/assets/Achievements/shield.png",
        description: "Activate double authentification",
        progress: () => {
			return props.userProp.achievements & 32 ? 1 : 0;
		},
		current: -1,
        max: 1,
    },
]
</script>

<template>
    <h2 style="text-align: center; " class="m-5 HoverTitle" @click="toggleFriends">Achievements </h2>
    <div class="card text-white bg-dark overflow-auto shadow-lg m-5" style="max-width: 100vw; max-height: 70vh;">
        <template v-for="(achievement, index) in gameProgressAchievements" :key="index">
            <div v-if="displayAll">
                <AchievementDisplay @toggle-ach-display="toggleAchDisplay" :extendable=true :achiev-id="index" :achievement-prop="achievement" :achiev-progress="achievement.progress()"> </AchievementDisplay>
            </div>
            <div v-else>
                <template v-if="index == progressAchToDisplay">
                    <AchievementDisplay @toggle-ach-display="toggleAchDisplay" :extendable=true :achiev-id="index" :achievement-prop="achievement" :achiev-progress="achievement.progress()"> </AchievementDisplay>
                </template>
            </div>

        </template>
        <template v-for="(achievement, index) in achievementList" :key="index">
                <AchievementDisplay :extendable=false :achiev-id="index" :achievement-prop="achievement" :achiev-progress="achievement.progress()"> </AchievementDisplay>
        </template>
    </div>
</template>

<style scoped>
.HoverTitle:hover {
    cursor: pointer;
    text-decoration: underline;
}
</style>