<template>
    <div>
        <form class="new-course-maker" name="courseMaker" v-on:submit.prevent="createNewCourse">
            <h1>Add a Course</h1>

           
            <div class="form-input-group">
                <label for="course-name">Course Name</label>
                <input type="text" id="course-name" v-model="course.courseName" required autofocus />
            </div>

            <div class="form-input-group">
                <label for="street-address">Street Address </label>
                <input type="text" id="street-address" v-model="course.streetAddress" required autofocus />
            </div>

            <div class="form-input-group">
                <label for="city-name">City </label>
                <input type="text" id="city-name" v-model="course.city" required autofocus />
            </div>

            <div class="form-input-group">
                <label for="state-name">State</label>
                <input type="text" id="state-name" v-model="course.state" required autofocus />
            </div>

            <div class="form-input-group">
                <label for="zip-code">Zip</label>
                <input type="text" id="zip-code" v-model="course.zipCode" required autofocus />
            </div>

            <button class="submitBtn" type="submit">create new course</button>
        </form>
    </div>
</template>


<script>
import CourseService from '../services/CourseService.js'

export default {
    data () {
        return {
            course: {
                courseName: '',
                streetAddress: '',
                city: '',
                state: '',
                zipCode: ''
            },
            
        }
    },

    methods: {
        createNewCourse() {
            CourseService
            .createCourse(this.course)
            .then((response) => {
                    if (response.status == 201) {
                        alert('Course added'),
                        document.courseMaker.reset();
                        this.$router.push({
                            path: '/admin',
                            query: { registration: 'success' },
                        });
                    }
                })
                .catch((error) => {
                    const response = error.response;
                    this.registrationErrors = true;
                    if (response.status === 400) {
                        this.registrationErrorMsg = 'Bad Request: Validation Errors';
                    }
                });
            }
        },
        created() {

        CourseService
            .getAllCourses()
            .then((response) => this.courses = response.data)
        }
}
</script>

<style scoped>
.new-course-maker {
    background-color: darkkhaki;
}
</style>