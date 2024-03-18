<script lang="ts" setup>
import type IStudent from '@/interfaces/IStudent';
import { useStudentStore } from '@/stores/student';
import { onMounted, ref, type Ref} from 'vue';

const studentStore = useStudentStore();
const students:Ref<IStudent[]> = ref([]);

onMounted(async () => {
    await studentStore.getStudents();
    students.value = studentStore.students;
    console.log('Vista:'+students.value);
});

const deleteProject = async(id: number) => {
    const confirmDelete = confirm('¿Estás seguro de eliminar este estudiante?');
    if(confirmDelete){
        await studentStore.deleteStudent(id.toString());
        students.value = studentStore.students;
    }
};
</script>

<template>
    <ol class="list">
        <div v-for="(student, index) in students" :key="index" class="list-item">
            <li >
                <RouterLink :to="{ name: 'student', params: { id: student.id } }">
                    {{ student.name }}
                </RouterLink>
            </li>
            <button class="delete" v-on:click="deleteProject(student.id)">Eliminar</button>
        </div>
    </ol>
</template>

<style scoped>
.list {
    counter-reset: li;
    width: 100%;
    list-style: none;
    font: 1.5rem 'trebuchet MS', 'lucida sans';
    padding: 0;
    margin: 1rem;

    & .list-item {
        display: flex; 
        align-items: center;
        justify-content: space-between;
        gap: 3rem;
        margin: 1rem;
    }

    & li {
        position: relative;
        display: block;
        width: 100%;
        padding: .4rem .4rem .4rem 2rem;
        margin: 1rem 0;
        background: #ddd;
        color: #34495E;
        text-decoration: none;
        border-radius: .3rem;
        transition: all .3s ease-out;

        &:hover {
            background: #34495E;
        }

        &:hover:before {
            transform: rotate(360deg);
        }

        &:before {
            content: counter(li);
            counter-increment: li;
            position: absolute;
            left: -1.3rem;
            top: 50%;
            color: black;
            margin-top: -1.6rem;
            background: #41B883;
            height: 3rem;
            width: 3rem;
            line-height: 2.5rem;
            border: .3rem solid #fff;
            text-align: center;
            font-weight: bold;
            border-radius: 2rem;
            transition: all .3s ease-out;
        }
    }
}
button {
  --width: 150px;
  --timing: 1s;
  border: 0;
  width: var(--width);
  padding-block: 1rem;
  color: #fff;
  font-weight: bold;
  font-size: 1.2rem;
  transition: all 0.2s;
  border-radius: .5rem;
  cursor: pointer;
}

.delete{
  background: rgb(250, 82, 82);
  &:hover{
    background-image: linear-gradient(
    to right,
    rgb(241, 25, 25),
    rgb(241, 25, 25) 25%,
    rgb(255, 27, 27) 25%,
    rgb(241, 27, 27) 50%,
    rgb(253, 20, 20) 50%,
    rgb(253, 20, 20) 75%,
    rgb(255, 0, 0) 75%,
    rgb(255, 0, 0) 100%,
    rgb(241, 25, 25) 100%
  );
  animation: var(--timing) linear dance6123 infinite;
  transform: scale(1.1) translateY(-1px);
  }
}
@keyframes dance6123 {
  to {
    background-position: var(--width);
  }
}
</style>