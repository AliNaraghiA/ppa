<template>
    <div class="exclusive">
        <div class="enExclusive mont">
            <Header lang="eng" @change="change" />
            <div class="topDiv">
                <h3 class="mont">Products</h3>
                <h1>exclusive Products</h1>
                <p>
The ability to calculate, design, and manufacture molds, as well as consultation regarding casting and production.
                </p>
            </div>
            <div class="lineDiv">
                <Lines myVw="1920" myVh="100" />
            </div>
            <div class="formText">
                To place an order for custom profiles, please complete the form below. Our team will contact you as soon as possible for further guidance
            </div>
            <form @submit.prevent="submitForm">
                <div class="d-flex justify-content-between flex-wrap">
                    <input
                        type="text"
                        name="name"
                        v-model="name"
                        placeholder="Name*" />
                    <input
                        type="text"
                        name="work"
                        v-model="work"
                        placeholder="Field of Work " />
                    <input
                        type="tell"
                        name="tell"
                        v-model="tell"
                        placeholder="Telephone Numbers *" />
                    <input
                        type="email"
                        name="email"
                        v-model="email"
                        placeholder="Email" />
                    <textarea
                        name="message"
                        v-model="message"
                        rows="4"
                        placeholder="Message"></textarea>
                </div>
<!--                 <el-upload
                    ref="attachment"
                    @change="handleAttachment"
                    class="upload-demo"
                    drag
                    action="https://jsonplaceholder.typicode.com/posts/"
                    :on-preview="handlePreview"
                    :on-remove="handleRemove"
                    multiple>
                    <div class="el-upload__text">
                        فایل را اینجا رها کنید یا برای آپلود کلیک کنید
                        <img src="/icons/export.svg" alt="" />
                    </div>
                </el-upload>
                <div class="size">
                    Attachment cannot exceed 
                    <span> 5 MB   </span>
                    
                </div>
                <button class="effectBtn" type="submit">
                    <span>SEND</span>
                </button> -->
                          <!--  -->
          <input type="file" ref="attachment" @change="handleAttachment" />
                     <div class="size">Attachment cannot exceed <span> 5 MB </span></div> 
          <!--  -->
          <button type="submit" class="effectBtn">
            <span> SEND </span>
          </button>
            </form>
            <div
                style="color: red; ,font-size:flex-direction: ;;"
                v-if="successMessage"
                class="success-message">
                {{ successMessage }}
            </div>
            <div class="footer">
                <Footer lang="en" />
            </div>
        </div>
        <MouseEffect />
    </div>
</template>

<script>
import axios from "axios";
export default {
    data() {
        return {
            language: "",
            name: "",
            work: "",
            tell: "",
            email: "",
            message: "",
            attachment: null,
            successMessage: "",
        };
    },
    mounted() {
        this.language = this.$store.state.lang;
    },
    methods: {
        change() {
            this.$store.commit("change");
        },
        handleAttachment(event) {
            this.attachment = event.target.files[0];
        },
        async submitForm() {
            const apiUrl =
                "https://backend.ppa-co.ir/wp-json/contact-form-7/v1/contact-forms/388/feedback";

            const formData = new FormData();
            formData.append("your-name", this.name);
            formData.append("work", this.work);
            formData.append("tell", this.tell);
            formData.append("your-email", this.email);
            formData.append("your-message", this.message);
            formData.append("your-file2", this.attachment);

            try {
                const response = await axios.post(apiUrl, formData, {
                    headers: { "Content-Type": "multipart/form-data" },
                });
                if (response.data.status === "mail_sent") {
                    this.successMessage = "Your message was sent successfully!";
                } else {
                    this.successMessage =
                        "Failed to send your message. Please try again.";
                }
            } catch (error) {
                console.error(error);
                this.successMessage =
                    "Failed to send your message. Please try again.";
            }
        },
    },
    watch: {
        "$store.state.lang"() {
            this.language = this.$store.state.lang;
        },
    },
};
</script>
