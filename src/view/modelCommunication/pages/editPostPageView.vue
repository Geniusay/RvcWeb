<script lang="ts" setup>
import editorComponent from '@/components/editor/editorComponent.vue'
import "@/assets/css/post/postContent.css"
import { GetPostDetailsForm, PostForm, RvcCommunicationPostType } from '@/api/post/postType'
import { getPostType, postAdd, getPostDetails, uploadPicture } from '@/api/post/postApi'
import { ref } from 'vue'
import router from '@/router'
import { message } from '@/utils/message'
let tagsOption = ref<{
    value: string | undefined
    label: string | undefined
}[]>([])
let postForm = ref<PostForm>({
    title: '',
    content: '',
    coverId: '',
    coverUrl: '',
    tagId: ''
})
let oldPost = ref<{
    title: string,
    content: string,
    coverId: string | null,
    coverUrl: string | null,
    tagId: string,
    tagName: string
}>({
    title: '',
    content: '',
    coverId: '',
    coverUrl: '',
    tagId: '',
    tagName: ''
})
getPostType().then(res => {
    let data = <RvcCommunicationPostType[]>(res.data)
    for (let i = 0; i < data.length; i++) {
        tagsOption.value.push({
            value: data[i].id,
            label: data[i].tagName
        })
    }
})
let formWarning = ref<{
    title: boolean,
    content: boolean,
    cover: boolean,
    tag: boolean
}>({
    title: false,
    content: false,
    cover: false,
    tag: false
})
let uploadCoverLoading = ref(false)
let typeSelectvisibility = ref(false)
let clickType = ref(false)
let currentTypeIndex = ref(-1)
let uploadFailed = ref(false)
let saveDisabled = ref(false)
const handleClickSort = function () {
    clickType.value = true
    typeSelectvisibility.value = !typeSelectvisibility.value
    setTimeout(function () {
        clickType.value = false
    }, 200)
}
const handleBlur = function () {
    setTimeout(function () {
        typeSelectvisibility.value = false
    }, 200)
}
const postHasChanged = function () {
    return !(postForm.value.content == oldPost.value.content && postForm.value.title == oldPost.value.title && postForm.value.tagId == oldPost.value.tagId && postForm.value.coverId == oldPost.value.coverId)
}
const savePost = function () {
    if (saveDisabled.value) {
        return
    }
    if (!postHasChanged()) {
        return
    }
    if (check()) {
        formWarning.value.content = postForm.value.content == '<p><br></p>'
        formWarning.value.title = postForm.value.title == ''
        formWarning.value.cover = postForm.value.coverUrl == ''
        formWarning.value.tag = postForm.value.tagId == ''
        setTimeout(function () {
            formWarning.value.content = false
            formWarning.value.title = false
            formWarning.value.cover = false
            formWarning.value.tag = false
        }, 300)
        return
    }
    postForm.value.postId = (router.currentRoute.value.query.postId as string)
    saveDisabled.value = true
    postAdd(postForm.value).then((res: any) => {
        if (res.code == 200) {
            message.success('保存成功')
            router.back()
        } else {
            message.error(res.msg)
            saveDisabled.value = false
        }
    })
}
const check = function () {
    return postForm.value.title == '' || postForm.value.tagId == '' || postForm.value.content == '<p><br></p>' || postForm.value.coverId == ''
}
const handleCoverSuccess = function () { }
const beforeCoverUpload = function (rawFile: File) {
    if ((rawFile.size / (1024 * 1024)) > 10) {
        message.warning('请上传小于20M的图片')
        return false
    }
    uploadCoverLoading.value = true
    setTimeout(function () {
        if (postForm.value.coverUrl == '') {
            uploadCoverLoading.value = false
            uploadFailed.value = true
        }
    }, 100000)
    uploadPicture(rawFile).then((res: any) => {
        if (res.code == 200) {
            uploadCoverLoading.value = false
            postForm.value.coverId = res.data.id
            postForm.value.coverUrl = res.data.url
            message.success('上传成功')
        } else {
            uploadCoverLoading.value = false
            uploadFailed.value = true
            message.error('上传失败')
        }
    })
    return false
}
const getContent = function (html: string) {
    postForm.value.content = html
}

const loadOldPost = function () {
    let form = ref<GetPostDetailsForm>({
        postId: (router.currentRoute.value.query.postId as string)
    })
    getPostDetails(form.value).then((res: any) => {
        if (res.code == 200) {
            let data = res.data
            oldPost.value.title = data.title
            oldPost.value.content = data.content
            oldPost.value.tagId = data.postType.id
            oldPost.value.coverUrl = data.cover
            oldPost.value.coverId = data.coverId
            oldPost.value.tagName = data.postType.tagName
            postForm.value.title = oldPost.value.title
            postForm.value.content = oldPost.value.content
            postForm.value.tagId = oldPost.value.tagId
            postForm.value.coverUrl = oldPost.value.coverUrl
            postForm.value.coverId = oldPost.value.coverId
            postForm.value.tagName = oldPost.value.tagName
        } else {
            message.error(res.msg)
        }

    })

}
loadOldPost()

</script>
<template>
    <div class="editPost-page">
        <div class="editPost-page__center">
            <div class="editPost-page__center__left">
                <div class="label">
                    标题
                </div>
                <div style="width: 100%;height: 30px;">
                    <input class="input" placeholder="标题" :class="formWarning.title ? 'formWarning' : 'formDefault'"
                        v-model="postForm.title">
                </div>
                <div class="label">
                    内容
                </div>
                <div style="width: 90%; border-radius: 10px;" :class="formWarning.content ? 'formWarning' : 'formDefault'">
                    <editorComponent v-if="postForm.content" :getContent="getContent" :editorContent="postForm.content">
                    </editorComponent>
                </div>
            </div>

        </div>
        <div class="editPost-page__center__right">
            <!-- <div class="label">
              预览
          </div>
          <div class="post-content editPost-page__center__right__Preview" v-html="content">

          </div> -->
            <div class="button-group">
                <div class="button-group__submit" @click="savePost"
                    :style="{ cursor: !postHasChanged() ? 'not-allowed' : 'pointer' }">
                    保存
                </div>
            </div>
            <div class="label">
                封面
            </div>
            <div>
                <el-upload ref="uploadAudioRef" class="upload-demo"
                    :class="formWarning.tag ? 'el-formWarning' : 'el-formDefault'" drag :auto-upload="true"
                    :on-success="handleCoverSuccess" :before-upload="beforeCoverUpload">
                    <div class="loadding" v-if="uploadCoverLoading"></div>
                    <div class="error" v-if="uploadFailed && !postForm.coverUrl">×</div>
                    <img v-if="postForm.coverUrl" style="width: 100%;" :src="postForm.coverUrl" />
                    <div class="el-upload__text">
                        将文件拖拽到此处或点击上传
                    </div>
                    <div class="el-upload__text">
                        最多可上传小于20M的图片
                    </div>
                </el-upload>
            </div>
            <div class="label">
                标签
            </div>
            <div style="text-align: left;">
                <div tabindex="-1" class="type-selecter"
                    :style="{ border: typeSelectvisibility ? 'rgba(24,100,171) 1px solid' : '' }"
                    :class="[clickType ? 'dither-animation' : '', formWarning.tag ? 'formWarning' : 'formDefault']"
                    @click="handleClickSort" @blur="handleBlur">
                    <div class="horizontal-center" style="display: flex;">
                        <span style="line-height: 40px;margin-left: 3px;width: 300px;">{{
                            currentTypeIndex != -1 ? tagsOption[currentTypeIndex]?.label : oldPost?.tagName }}</span>
                        <span>
                            <img width="14" height="14" class="vertical-center" style="transition: all 0.2s;"
                                :class="typeSelectvisibility ? 'revolve-animation' : ''" src="/icon/arrow-down.svg">
                        </span>
                    </div>

                </div>
                <div class="type-select" v-show="typeSelectvisibility">
                    <div class="type-select__item" v-for="(tag, index) in tagsOption" :key="index"
                        @click="currentTypeIndex = index; typeSelectvisibility = false; postForm.tagId = tagsOption[currentTypeIndex]?.value!">
                        {{ tag.label }}
                    </div>
                </div>
            </div>

        </div>
    </div>
</template>
<style scoped>
.editPost-page {
    position: relative;
    height: 100vh;
    width: 100%;
    background-color: rgba(26, 27, 30);
}

.upload-demo {
    position: relative;
    width: 100%;
    max-height: 400px;
  overflow: scroll;
}

.loadding {
    position: relative;
    left: 50%;
    transform: translate(-50%);
    height: 34px;
    width: 34px;
    border-radius: 17px;
    background-color: rgba(44, 46, 51);
    font-size: 20px;
    line-height: 36px;
    color: white;
    font-weight: 700;
    border-top: rgba(25, 113, 194) 1px solid;
    margin-bottom: 20px;
    animation: roll 1s linear infinite;
}

@keyframes roll {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}

.error {
    position: relative;
    left: 50%;
    transform: translate(-50%);
    height: 36px;
    width: 36px;
    border-radius: 18px;
    background-color: rgba(44, 46, 51);
    font-size: 20px;
    line-height: 36px;
    color: white;
    font-weight: 700;
    margin-bottom: 20px;
}

.editPost-page__center {
    position: relative;
    width: 80%;
    height: 95%;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    display: flex;
}

.editPost-page__center__left {
    position: relative;
    width: 60%;
    overflow: scroll;
    margin: 30px 0;
}

.editPost-page__center__right {
    height: 100%;
    width: 35%;
    max-width: 400px;
    position: fixed;
    top: 150px;
    left: calc(40% + 450px);

}

.editPost-page__center :deep(.el-breadcrumb__inner) {
    color: white;
}

.cover-uploader .cover {
    width: 100%;
    height: 178px;
    display: block;
}

.cover-uploader {
    display: flex;
    justify-content: left;
}

:deep(.cover-uploader .el-upload) {
    border: 1px dashed grey;
    border-radius: 6px;
    width: 100%;
    cursor: pointer;
    position: relative;
    overflow: hidden;
    background-color: rgba(37, 38, 43);
    transition: var(--el-transition-duration-fast);
}

:deep(.cover-uploader .el-upload:hover) {
    border-color: grey;
}

:deep(.el-icon.cover-uploader-icon) {
    font-size: 28px;
    color: #8c939d;
    width: 100%;
    height: 178px;
    text-align: center;
}

.label {
    display: block;
    font-size: 16px;
    color: rgba(255, 255, 255, 0.7);
    font-weight: 700;
    text-align: left;
    margin-top: 10px;
    margin-bottom: 5px;
}

.type-selecter {
    height: 40px;
    padding: 0 20px;
    border-radius: 5px;
    background-color: rgba(37, 38, 43);
    cursor: pointer;
    display: flex;
    color: white;
    transition: all 0.3s;
    user-select: none;
    border: rgba(55, 58, 64) 1px solid;
}

.type-select {
    position: absolute;
    margin-top: 10px;
    width: 340px;
      border-radius: 10px;
    border: rgba(55, 58, 64) 1px solid;
    background-color: rgba(37, 38, 43);
    padding: 5px;
    z-index: 10;
    max-height: 100px;
  overflow: scroll;
    user-select: none;
}

.type-select__item {
    position:relative;
    padding-left: 15px;
    width: calc(100% - 15px);
    height: 40px;
    line-height: 40px;
    font-size: 14px;
    text-align: left;
    border-radius: 10px;
    color: rgba(255, 255, 255, 0.7);
}

.type-select__item:hover {
    background-color: rgba(56, 58, 64);
    cursor: pointer;
}

.input {
    position: relative;
    width: 90%;
    float: left;
    height: 30px;
    border: rgba(100, 100, 100) 1px solid;
    outline: none;
    border-radius: 5px;
    background-color: rgba(37, 38, 43);
    color: rgba(255, 255, 255, 0.7);
    padding-left: 10px;
}

.editPost-page__center__right__Preview {
    height: 700px;
    width: calc(100% - 20px);
    padding: 10px;
    border: rgba(255, 255, 255, 0.4) 1px solid;
    border-radius: 10px;
}

.button-group {
    position: relative;
    /* height: 50px; */
    width: 100%;

}

.button-group__save {
    position: relative;
    height: 40px;
    width: 100%;
    border-radius: 5px;
    top: 50%;
    left: 100%;
    transform: translate(-100%, -50%);
    background-color: rgba(37, 38, 43);
    color: white;
    font-size: 16px;
    /* font-weight: 700; */
    line-height: 40px;
    cursor: pointer;
    user-select: none;
    margin-top: 10px;
    transition: all 0.3s;
}

.button-group__submit {
    position: relative;
    height: 40px;
    width: 100%;
    border-radius: 5px;
    top: 50%;
    left: 100%;
    transform: translate(-100%, -50%);
    background-color: rgba(0, 122, 204);
    color: white;
    font-size: 16px;
    /* font-weight: 700; */
    line-height: 40px;
    cursor: pointer;
    user-select: none;
    margin-top: 10px;
    transition: all 0.3s;
}

.dither-animation {
    top: 36px;
}

.revolve-animation {
    transform: rotateZ(180deg);
    transform-origin: 6px 3.5px;
}

:deep(.upload-demo *) {
    background-color: transparent;
}

.button-group__submit:hover {
    background-color: rgba(20, 142, 224);
}

.formWarning {
    border: rgba(224, 49, 49) 1px solid;
}

:deep(.el-formWarning .el-upload-dragger) {
    border: rgba(224, 49, 49) 1px dashed;
}

.formDefault {
    border: rgba(55, 58, 64) 1px solid;
}

:deep(.el-formDefault .el-upload-dragger) {
    border: rgba(55, 58, 64) 1px dashed;
}</style>