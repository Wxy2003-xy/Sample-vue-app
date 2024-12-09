<template lang="">
    <div>
        
    </div>
</template>
<script lang="ts">
interface Slot {
    title: string;
    startTime?: string;
    endTime?: string;
    day?: string;
    color?: string;
    weeks?: string;
    venue?: string;
    lessonType?: string
}

export default {
    name: "Timetable",
    components: {},
    data() {
        const data = localStorage.getItem('cachedTimeSlots');
        return {
            timeSlots: data ? JSON.parse(data):[],
            slots: [] as Slot[],
            
        }
    },
    mounted() {
        console.log("mounted")
    },
    computed: {
        async fetchTimeSlotInfo(acadYear:string, moduleCode: string, semester:number) {
            const apiUrl = `https://api.nusmods.com/v2/${acadYear}/modules/${moduleCode}.json`;
            try {
                const response = await fetch(apiUrl);
                if (!response) {
                    throw new Error('network');
                }
                const data = await response.json();
                const semesterSpecificInfo = data.semesterData.find(semesterData => semesterData.semester === semester);
                if (!semesterSpecificInfo) {
                    throw new Error('Semester information not found');
                }
                return semesterSpecificInfo.timetable.map(slot => ({
                    classNo: slot.classNo,
                    startTime: slot.startTime,
                    endTime: slot.endTime,
                    weeks: slot.weeks,
                    venue: slot.venue,
                    day: slot.day,
                    lessonType: slot.lessonType,
                    title: moduleCode
                }));
            } catch (e) {
                console.log("err");
            }
        },
    },
    methods: {

    },
}
</script>
<style lang="">
    
</style>