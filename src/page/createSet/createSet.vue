<template>
  <div class="training">
    <div class="step-bar">
      <el-steps :active="currentStep" simple>
        <el-step title="创建样本集" icon="el-icon-picture"></el-step>
        <el-step title="选择样本" icon="el-icon-edit"></el-step>
        <el-step title="分类映射" icon="el-icon-upload"></el-step>
        <!--<el-step title="模型训练" icon="el-icon-picture"></el-step>-->
      </el-steps>
    </div>
    <div class="steps" :class="containerClass">
      <div v-if="currentStep === 1" class="type step">
        <div class="title flex-c-row">
          <span class="step-title">
            设置样本集名称和描述
          </span>
          <el-button size="mini" type="primary" class="title-btn" @click="exit">退出</el-button>
        </div>
        <!--确定训练类别-->
        <div class="cards-container">
          <div class="card-container card-card" :class="addSessionParams.trainType !== ''?'card1-selected':''">
            <el-card shadow="always" class="type-card card1">
              <div class="card-img">
                <svg t="1606187640296" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="12686" width="100" height="100"><path d="M511.782075 51.288182c14.696614 0 29.393227 0.699839 43.989864 2.099516 14.49666 1.399677 28.893342 3.499194 43.190048 6.298549 14.296706 2.799355 28.493435 6.198572 42.390233 10.297627 6.89841 1.999539 13.796821 4.199032 20.595254 6.59848 3.399217 1.199724 6.89841 2.399447 10.297627 3.699148 0.599862 0.199954 1.299701 0.499885 1.899563 0.699838 0.399908 0.199954 0.899793 0.499885 1.2997 0.499885 0.799816 0 0.899793-1.699608 1.199724-2.399447 5.198802-13.796821 10.397604-27.493665 15.596406-41.290486 0.499885-1.399677 1.099747-2.799355 1.599631-4.199032-17.196038-6.498503-34.692006-12.097213-52.487905-16.79613-17.895876-4.698917-35.991707-8.398065-54.287491-11.097443-18.395761-2.799355-36.991476-4.498963-55.587192-5.198802-18.895646-0.699839-37.791292-0.399908-56.686938 0.999769-18.695692 1.399677-37.391384 3.799125-55.787146 7.198342-18.09583 3.399217-36.091684 7.698226-53.687629 12.997005-17.395992 5.198802-34.592029 11.397374-51.388159 18.495738-16.696153 6.998387-33.092375 14.996544-48.988712 23.694541-15.79636 8.697996-31.092836 18.195807-45.789449 28.493434-14.696614 10.19765-28.793365 21.195116-42.290256 32.892421-3.399217 2.899332-6.798434 5.898641-10.097673 8.997927 8.89795 9.697765 17.695923 19.395531 26.593872 29.093296l6.59848 7.198341c0.399908 0.499885 0.99977 1.699608 1.699608 1.299701 0.699839-0.399908 1.199724-1.199724 1.799585-1.699608 4.698917-4.299009 9.597788-8.498042 14.49666-12.597098 9.697765-8.098134 19.795439-15.696383 30.093066-22.994701 10.397604-7.298318 21.095139-14.096752 31.992628-20.495278 10.997466-6.398526 22.194886-12.397143 33.692237-17.895876 11.597328-5.59871 23.494586-10.597558 35.591799-15.196499 12.097213-4.59894 24.294402-8.598019 36.691545-12.197189 12.49712-3.599171 25.094218-6.59848 37.891269-9.097904 12.897028-2.499424 25.894033-4.498963 38.991016-5.898641 12.997005-1.399677 26.193964-2.199493 39.290947-2.499424h9.597788zM53.187744 470.491589c-13.196959-1.199724-26.493895-2.399447-39.690854-3.59917-3.699148-0.299931-7.498272-0.699839-11.19742-0.99977C0 490.88689-0.399908 515.981108 0.99977 541.075325c1.399677 24.994241 4.698917 49.788528 9.697765 74.282884 4.998848 23.894494 11.597328 47.489058 19.895416 70.483759 8.298088 22.894725 18.195807 45.289564 29.693158 66.784612 11.497351 21.595024 24.494356 42.290255 38.891039 61.985717 1.699608 2.29947 3.399217 4.698917 5.198802 6.998388 10.597558-7.998157 21.095139-15.996314 31.692697-23.994472 2.499424-1.899562 4.998848-3.899102 7.598249-5.698687 0.599862-0.399908 1.699608-0.799816 1.199724-1.699608-0.599862-1.099747-1.599631-1.999539-2.29947-2.999309-3.799125-5.098825-7.498272-10.297627-11.097443-15.596406-14.196729-20.895185-26.693849-42.890117-37.391385-65.784842-10.697535-23.094679-19.595485-47.08915-26.293941-71.583506-6.89841-24.794287-11.597328-50.288413-14.196729-75.882515-2.599401-26.093987-2.999309-52.387929-1.099746-78.481916 0.199954-3.199263 0.399908-6.298549 0.699838-9.397835zM512.28196 972.675876c-18.295784 0-36.491592-1.099747-54.687399-3.29924-8.997927-1.099747-17.895876-2.399447-26.693849-3.999078-0.299931-0.099977-1.499654-0.499885-1.799585-0.299931-0.399908 0.199954-0.399908 1.499654-0.499885 2.099516-0.299931 2.099516-0.799816 4.199032-1.199724 6.398526-0.799816 4.398986-1.599631 8.697996-2.399447 13.096982-1.799585 9.597788-3.599171 19.195577-5.298779 28.793365 24.494356 4.498963 49.288643 7.298318 74.182907 8.198111 24.994241 0.899793 49.988482 0 74.882745-2.799355 24.594333-2.799355 48.988712-7.298318 72.983184-13.596867 23.794517-6.198572 47.08915-14.196729 69.683943-23.794517 22.094909-9.397835 43.489979-20.295324 64.085233-32.59249 2.599401-1.599631 5.198802-3.099286 7.698227-4.698917-5.398756-8.697996-10.897489-17.395992-16.296245-26.093988-2.699378-4.398986-5.398756-8.697996-8.198111-13.096982-0.599862-0.99977-1.299701-1.999539-1.899563-2.999309-0.399908-0.599862-0.499885-1.499654-1.2997-0.999769-1.699608 0.99977-3.29924 2.099516-4.998848 3.099286-17.196038 10.397604-34.991937 19.695462-53.387699 27.793595-18.495738 8.098134-37.491361 14.996544-56.886892 20.695232-19.495508 5.698687-39.390924 10.097673-59.386316 13.096982-20.295324 3.099286-40.690624 4.798894-61.185902 5.098825-2.499424-0.099977-4.998848-0.099977-7.398295-0.099977zM1004.768481 373.613912c-6.998387-24.894264-15.896337-49.188666-26.593872-72.683252-2.999309-6.59848-6.098595-13.096982-9.397834-19.595485-13.396913 6.698457-26.693849 13.49689-40.090763 20.195346-1.399677 0.699839-2.899332 1.499654-4.398986 2.199494-0.299931 0.099977-1.099747 0.299931-1.199724 0.699838-0.099977 0.299931 0.299931 0.799816 0.399908 0.99977 1.499654 2.799355 2.899332 5.59871 4.29901 8.498042 3.199263 6.59848 6.098595 13.296936 8.997926 19.995392 5.59871 13.396913 10.597558 26.99378 14.996545 40.890578 4.299009 13.896798 7.998157 27.99355 10.997466 42.290256 2.999309 14.396683 5.398756 28.893342 6.998387 43.489979 1.699608 14.696614 2.599401 29.593181 2.799355 44.389772 0.199954 14.896568-0.299931 29.793135-1.499654 44.689702-1.199724 14.796591-3.199263 29.593181-5.798664 44.289795a400.907623 400.907623 0 0 1-4.498964 21.794978c-0.799816 3.599171-1.699608 7.098364-2.599401 10.597558-0.499885 1.799585-0.899793 3.599171-1.399677 5.398756-0.299931 1.199724-0.799816 2.29947-0.99977 3.499194-0.199954 0.99977 0.99977 0.899793 1.699609 1.099747 1.599631 0.299931 3.199263 0.899793 4.698917 1.2997l10.797512 2.999309c7.198341 1.999539 14.49666 3.999079 21.695001 6.098595 3.399217 0.99977 6.798434 1.899562 10.19765 2.899332 4.898871-17.395992 8.797973-35.091914 11.797282-52.987791s5.098825-35.991707 6.198572-54.187514c1.099747-18.295784 1.199724-36.691546 0.299931-54.98733-0.899793-18.295784-2.699378-36.491592-5.59871-54.587422-2.799355-17.795899-6.498503-35.491822-11.19742-52.887813-0.399908-2.099516-0.99977-4.199032-1.599632-6.398526zM419.703292 815.412113c19.695462 6.098595 40.090762 10.297627 60.58604 12.49712 20.695231 2.199493 41.590417 2.399447 62.385625 0.499885 11.697305-0.99977 23.294632-2.699378 34.791983-5.098825l-3.899101-19.495508c-0.599862-3.199263-1.299701-6.398526-1.899563-9.497811-0.299931-1.599631-0.599862-3.099286-0.899792-4.698918-0.199954-0.899793 0-1.899562-1.099747-1.799585-0.899793 0.099977-1.799585 0.399908-2.699378 0.599862l-4.798894 0.899792c-3.29924 0.599862-6.59848 1.099747-9.89772 1.499655-6.498503 0.899793-12.997005 1.499654-19.495507 1.999539-13.096982 0.899793-26.293941 0.799816-39.390924-0.099977-12.997005-0.899793-25.99401-2.799355-38.691085-5.498733-12.49712-2.699378-24.794287-6.198572-36.791522-10.597558-11.897259-4.299009-23.394609-9.397835-34.59203-15.296475-11.097443-5.798664-21.794978-12.397143-32.092605-19.695462-10.097673-7.198341-19.795439-15.096521-28.793365-23.594563-9.197881-8.598019-17.795899-17.795899-25.794057-27.593642-0.99977-1.199724-1.999539-2.499424-2.999309-3.799125-0.599862-0.699839-1.099747-1.599631-1.699608-2.29947-0.299931-0.299931-0.199954-0.299931-0.499885-0.199954-0.399908 0.099977-0.99977 0.699839-1.2997 0.99977l-3.899102 2.999309-15.896337 12.297166c-2.499424 1.899562-4.998848 3.899102-7.498272 5.798664 7.698226 9.997696 15.996314 19.395531 24.794286 28.393457s18.195807 17.395992 27.99355 25.294172c9.797742 7.89818 20.195347 15.196498 30.892882 21.894955 10.797512 6.698457 21.894955 12.797051 33.392306 18.09583 11.597328 5.398756 23.494586 10.097673 35.591799 14.096752 1.199724 0.499885 2.699378 0.899793 4.199032 1.399678zM514.081545 230.646854v-20.095369-9.997697c0-1.599631-0.099977-3.199263 0-4.798894 0-0.899793 0.199954-1.399677-0.899792-1.499654-0.899793-0.099977-1.899562 0.099977-2.799355 0.099977-1.499654 0-3.099286 0-4.598941 0.099977-3.29924 0.099977-6.59848 0.199954-9.897719 0.399908-6.698457 0.399908-13.396913 0.99977-19.995393 1.799585-13.096982 1.599631-26.093987 3.999079-38.791061 7.098364-12.597097 3.099286-24.994241 7.098364-37.091454 11.697305-11.897259 4.59894-23.494586 9.897719-34.791983 15.896337-11.19742 5.998618-22.094909 12.597097-32.492513 19.895416-10.297627 7.198341-20.195347 14.996544-29.593181 23.394609-9.397835 8.298088-18.295784 17.196038-26.593872 26.593872-8.398065 9.397835-16.196268 19.395531-23.39461 29.793136-7.198341 10.397604-13.796821 21.195116-19.695462 32.292559-1.499654 2.899332-2.999309 5.698687-4.398986 8.598019 5.498733 2.699378 10.897489 5.398756 16.396222 8.098134 2.699378 1.299701 5.398756 2.599401 7.998157 3.999078 2.29947 1.099747 4.798894 2.199493 6.998387 3.499194 0.599862 0.399908 1.099747 0.699839 1.499655 0.099977 0.399908-0.599862 0.599862-1.399677 0.99977-2.099516 1.199724-2.499424 2.599401-4.998848 3.899101-7.498273 2.699378-5.098825 5.59871-10.097673 8.697996-14.996544 11.997236-19.295554 26.393918-37.19143 42.690163-53.087768 16.296245-15.79636 34.492052-29.693158 54.087537-41.090532 19.995393-11.597328 41.390463-20.795208 63.585349-27.193734 22.794748-6.498503 46.289334-10.097673 69.983874-10.797512 2.499424-0.199954 5.398756-0.199954 8.198111-0.199954zM511.982029 389.210318v-30.692928c-22.39484 0-44.689703 4.898871-64.985026 14.396683-19.295554 8.997927-36.591569 21.994932-50.588343 37.991246-14.196729 16.196268-24.894264 35.291868-31.192813 55.887123-6.698457 21.695001-8.398065 44.78968-5.198802 67.184519 3.099286 21.695001 10.897489 42.590186 22.794748 60.985948 11.597328 17.995853 26.893803 33.492283 44.689702 45.289564 18.195807 11.997236 38.891039 20.09537 60.486063 23.394609 22.39484 3.499194 45.589495 2.099516 67.384473-4.299009 21.295093-6.198572 41.090532-17.196038 57.786685-31.792674 16.696153-14.596637 30.193043-32.892421 39.19097-53.187745 2.199493-4.898871 4.099055-9.997696 5.798664-15.096521-4.798894-1.599631-9.597788-3.099286-14.396683-4.698917-2.399447-0.799816-4.798894-1.499654-7.198342-2.299471-2.199493-0.699839-4.498963-1.599631-6.698456-2.199493-0.799816-0.199954-0.99977-0.399908-1.199724 0.399908-0.199954 0.799816-0.399908 1.499654-0.699838 2.29947-0.799816 2.199493-1.599631 4.299009-2.399448 6.398526-1.799585 4.398986-3.899102 8.697996-6.198571 12.797051-8.797973 15.79636-21.195116 29.593181-35.89173 40.090762-15.196498 10.797512-32.792444 17.995853-51.088228 21.09514-19.0956 3.199263-38.791062 1.799585-57.2868-3.999079-17.495969-5.498733-33.692237-15.096521-47.08915-27.693619-13.296936-12.49712-23.69454-27.893573-30.392997-44.789679-6.998387-17.795899-9.697765-37.19143-7.89818-56.287031 1.799585-18.695692 7.89818-36.791523 17.695923-52.787836 9.597788-15.496429 22.594794-28.793365 37.791292-38.691085 15.696383-10.19765 33.59226-16.696153 52.187974-18.895646 4.798894-0.499885 9.697765-0.799816 14.596637-0.799816z" fill="#515151" p-id="12687"></path><path d="M835.90739 206.852337c-12.697074 10.697535-25.394149 21.295093-38.091223 31.992628-12.797051 10.697535-25.594103 21.39507-38.291177 32.192583-3.599171 2.999309-7.198341 5.998618-10.797512 9.097903-1.799585 1.499654-3.499194 2.999309-5.298779 4.498964-0.899793 0.799816-1.799585 1.499654-2.699378 2.29947-0.299931 0.199954-0.499885 0.399908-0.699839 0.699838l-0.699838 0.499885c0 0.099977 0.299931 0.199954 0.399908 0.199954-0.099977 0.099977-1.299701-0.99977-1.399678-1.099746-0.699839-0.699839-1.299701-1.399677-1.999539-1.99954-1.599631-1.599631-3.199263-3.099286-4.798894-4.59894-3.199263-2.999309-6.398526-5.898641-9.697766-8.797973-4.898871 5.698687-9.797742 11.397374-14.79659 17.096061-2.399447 2.799355-4.798894 5.59871-7.198342 8.298088-0.499885 0.499885-1.199724 1.099747-1.499654 1.699608-0.299931 0.499885 0 0.599862 0.499885 0.99977 1.199724 0.99977 2.399447 2.099516 3.59917 3.199263 3.099286 2.799355 6.098595 5.59871 8.997927 8.498042-19.595485 16.496199-39.19097 32.892421-58.886431 49.38862-19.695462 16.596176-39.390924 33.092375-59.186363 49.68855-19.695462 16.496199-39.390924 32.992398-58.986408 49.488597-1.199724 0.99977-2.29947 1.999539-3.499194 2.899332-0.299931 0.299931-0.799816 0.799816-1.099746 0.99977-0.499885 0.199954-0.99977-0.199954-1.399678-0.399908-2.799355-1.199724-6.098595-1.699608-9.097904-2.199493-6.098595-0.799816-12.297166-0.599862-18.195807 0.699838-10.497581 2.29947-20.195347 8.098134-27.293711 16.096292-7.198341 8.098134-11.597328 18.495738-12.597097 29.193273-0.99977 11.397374 1.799585 22.994702 8.098134 32.492513 5.798664 8.89795 14.396683 15.996314 24.394379 19.795439 10.597558 4.099055 22.294863 4.59894 33.192352 1.2997 10.497581-3.099286 19.795439-9.597788 26.293941-18.395761 6.798434-9.097904 10.497581-20.395301 10.19765-31.792674 0-1.499654-0.099977-2.999309-0.299931-4.498964-0.099977-0.599862-0.099977-1.099747-0.29993-1.599631-0.099977-0.399908-0.399908-1.099747-0.299931-1.499655 0.099977-0.299931 0.399908-0.499885 0.699838-0.699838 0.599862-0.399908 1.099747-0.899793 1.699609-1.399678 1.299701-1.099747 2.499424-2.099516 3.799124-3.199263 4.898871-4.099055 9.797742-8.198111 14.596637-12.297166 9.797742-8.198111 19.595485-16.396222 29.29325-24.594333 19.695462-16.596176 39.490901-33.092375 59.186363-49.688551 19.595485-16.496199 39.290947-32.992398 58.886431-49.38862 4.59894-3.899102 9.197881-7.698226 13.796821-11.597328 10.19765 14.596637 19.0956 30.093066 26.393918 46.389311 7.498272 16.696153 13.296936 34.092144 17.495969 51.888044 4.199032 18.395761 6.59848 37.091453 7.098364 55.9871 0.499885 18.995623-0.899793 38.091223-4.199032 56.786915-0.799816 4.59894-1.799585 9.297858-2.799355 13.896798-0.399908 1.899562-0.799816 3.799125-1.399678 5.698687-0.199954 0.599862-0.399908 1.099747-0.499885 1.699608-0.099977 0.699839 0.099977 0.699839 0.799816 0.899793 2.099516 0.599862 4.299009 1.099747 6.498503 1.599631 2.399447 0.599862 4.698917 1.199724 7.098364 1.799586 4.698917 1.199724 9.297858 2.399447 13.996775 3.59917 2.29947 0.599862 4.498963 1.199724 6.798433 1.799586 3.399217-12.897028 5.898641-25.99401 7.59825-39.19097 1.699608-13.396913 2.599401-26.793826 2.699378-40.290716 0-13.596867-0.799816-27.093757-2.399447-40.590647-1.699608-13.296936-4.099055-26.393918-7.498273-39.390924-3.29924-12.797051-7.398295-25.394149-12.197189-37.691315-4.798894-12.19719-10.397604-23.994471-16.596176-35.491822-3.099286-5.798664-6.498503-11.397374-9.997696-16.996084-3.099286-4.898871-6.198572-9.997696-9.89772-14.596636-0.399908-0.499885-1.099747-0.99977-0.599862-1.599632 0.299931-0.399908 0.799816-0.599862 1.099747-0.799816l1.499654-1.199723c2.599401-2.199493 5.298779-4.398986 7.898181-6.59848 5.198802-4.398986 10.397604-8.697996 15.596406-13.096982 10.497581-8.797973 20.895185-17.595946 31.392766-26.293941 10.397604-8.697996 20.795208-17.495969 31.192813-26.193965 2.499424-2.099516 5.098825-4.299009 7.598249-6.398525-8.598019-10.19765-17.096061-20.395301-25.694079-30.592951-2.699378-2.899332-5.098825-5.698687-7.498273-8.598019z" fill="#515151" p-id="12688"></path></svg>
              </div>
              <div class="card-title">地物提取</div>
              <div class="card-desc">根据遥感影像中的光学特征识别影像中的地物类别</div>
            </el-card>
          </div>
          <div class="card-container">
            <el-form class="create-form demo-ruleForm" :model="addSessionParams" :rules="rules1" ref="ruleForm" label-width="100px">
              <el-form-item label="名称" prop="name">
                <el-input v-model="addSessionParams.name"></el-input>
              </el-form-item>
              <el-form-item label="描述" prop="desc">
                <el-input type="textarea" rows="2" v-model="addSessionParams.memo"></el-input>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="submitSetForm('ruleForm')">立即创建</el-button>
                <el-button @click="resetForm('ruleForm')">重置</el-button>
              </el-form-item>
            </el-form>
          </div>
        </div>
      </div>
      <div v-if="currentStep === 2" class="step">
        <div v-if="!showDetail">
          <div class="title flex-c-row">
            <span class="step-title">
              选择分类体系
            </span>
            <el-button size="mini" type="primary" class="title-btn" @click="back(1)">返回</el-button>
            <el-button size="mini" type="primary" class="title-btn" @click="exit">退出</el-button>
            <el-button size="mini" type="primary" class="title-btn">下一步</el-button>
          </div>
          <div class="">
            <div class="page page-classification main-content">
              <div class='query-tool'>
                <div class="search">
                  <el-input
                    placeholder="请输入内容"
                    v-model="category" @keyup.enter.native="getClassList">
                    <i slot="prefix" class="el-input__icon el-icon-search"></i>
                  </el-input>
                </div>
                <el-button type="primary" icon="el-icon-edit" @click="getClassList">查询</el-button>
                <el-button type="primary" icon="el-icon-edit" @click="handleAddClass">新增分类体系</el-button>
              </div>
              <div class="table-panel choose-panel">
                <el-table
                  :data="classList"
                  border
                  highlight-current-row
                  style="width: 100%">
                  <el-table-column
                    fixed="right"
                    label="操作"
                    min-width="150">
                    <template slot-scope="scope">
                      <el-button @click="handleEditClass(scope.row)" type="primary" size="small">编辑</el-button>
                      <!--            <el-button @click="handleClone(scope.row)" type="primary" size="small">克隆</el-button>-->
                      <!--            <el-button @click="handleFreeze(scope.row)" type="primary" size="small">冻结</el-button>-->
                      <el-button @click="handleDetail(scope.row)" type="primary " size="small">明细</el-button>
                    </template>
                  </el-table-column>
                  <el-table-column
                    prop="id"
                    label="ID"
                    min-width="40">
                  </el-table-column>
                  <el-table-column
                    prop="taonomyName"
                    label="体系名称"
                    min-width="120">
                  </el-table-column>
                  <el-table-column
                    prop="taonomyType"
                    label="类别"
                    min-width="100">
                  </el-table-column>
                  <el-table-column
                    prop="state_type"
                    label="状态"
                    min-width="80">
                  </el-table-column>
                  <el-table-column
                    prop="memo"
                    label="备注"
                    min-width="150">
                  </el-table-column>
                </el-table>
              </div>
              <el-dialog :title="formTitle" :visible.sync="classDialogFormVisible" width="400px">
                <el-form :model="classForm" ref="classForm" :rules="rules">
                  <el-form-item label="分类名称" :label-width="formLabelWidth" prop="taonomyName">
                    <el-input v-model="classForm.taonomyName" auto-complete="off"></el-input>
                  </el-form-item>
                  <el-form-item label="分类类别" :label-width="formLabelWidth" prop="taonomyType">
                    <el-radio v-model="classForm.taonomyType" label="1">地物分类</el-radio>
                    <el-radio v-model="classForm.taonomyType" label="2">变化检测</el-radio>
                  </el-form-item>
                  <el-form-item label="备注" :label-width="formLabelWidth">
                    <el-input type="textarea" placeholder="请输入内容" v-model="classForm.memo" :rows="4" maxlength="200" show-word-limit>
                    </el-input>
                  </el-form-item>
                </el-form>
                <div slot="footer" class="dialog-footer">
                  <el-button @click="classDialogFormVisible = false">取 消</el-button>
                  <el-button type="primary" @click="submitClassForm">确 定</el-button>
                </div>
              </el-dialog>
            </div>
          </div>
        </div>
        <!--设置明细-->
        <div v-if="showDetail">
          <div class="title flex-c-row">
            <span class="step-title">
              选择分类体系
            </span>
            <el-button size="mini" type="primary" class="title-btn" @click="back(2)">返回</el-button>
            <el-button size="mini" type="primary" class="title-btn" @click="exit">退出</el-button>
          </div>
          <div class="content">
            <div class="main-content category">
              <div class='query-tool'>
                <div class="search">
                  <!--<el-input-->
                  <!--placeholder="请输入内容"-->
                  <!--v-model="category" @keyup.enter.native="getCategoryList">-->
                  <!--<i slot="prefix" class="el-input__icon el-icon-search"></i>-->
                  <!--</el-input>-->
                  <el-input
                    placeholder="请输入内容"
                    v-model="category">
                    <i slot="prefix" class="el-input__icon el-icon-search"></i>
                  </el-input>
                </div>
                <el-button type="primary" icon="el-icon-edit" @click="getCategoryList">查询</el-button>
                <el-button type="primary" icon="el-icon-edit" @click="handleAddCategory">新增分类</el-button>
              </div>
              <div class="table-panel choose-panel">
                <div class="category-list" @drop.stop="handleDropList">
                  <ul class="search-tree" @drop.stop="handleDropList">
                    <li class="th-header">
                      <p>代码</p>
                      <p>类别</p>
                      <p class="td-opt">操作</p>
                    </li>
                    <li class="tr" v-for="item in categoryTable" :key="item.id" @dragstart="drag($event, item)" draggable="true">
                      <p v-html="item.id"></p>
                      <p v-html="item.label"></p>
                      <p class="td-opt">
                        <el-button @click="handleEditCategory(item)" type="primary" size="mini">编辑</el-button>
                        <el-button @click="handleDeleteCategory(item)" type="primary " size="mini">删除</el-button>
                      </p>
                    </li>
                  </ul>
                </div>
                <div class="flex-placeholder">
                </div>
                <div class="classify-tree">
                  <geo-tree :data="treeData" :handleDragOver="handleDragOver" @node-drag-start="nodeDragStart" default-expand-all draggable :handleDrop="handleDrop" :allowDrop="allowdrop"></geo-tree>
                </div>
                <div class="flex-placeholder center-buttom">
                  <el-button type="primary" @click="save">保存</el-button>
                </div>
              </div>
              <el-dialog :title="formTitle" :visible.sync="categoryDialogFormVisible" width="400px">
                <el-form :model="categoryForm" ref="categoryForm" :rules="rules">
                  <el-form-item label="分类名称" :label-width="formLabelWidth" prop="name">
                    <el-input v-model="categoryForm.name" auto-complete="off"></el-input>
                  </el-form-item>
                  <el-form-item label="分类编码" :label-width="formLabelWidth" prop="code">
                    <el-input v-model="categoryForm.code" auto-complete="off"></el-input>
                  </el-form-item>
                  <el-form-item label="填充颜色" :label-width="formLabelWidth" prop="fillColor">
                    <el-color-picker v-model="categoryForm.fillColor"></el-color-picker>
                  </el-form-item>
                  <el-form-item label="边框颜色" :label-width="formLabelWidth" prop="storkColor">
                    <el-color-picker v-model="categoryForm.storkColor"></el-color-picker>
                  </el-form-item>
                  <el-form-item label="分类图标" :label-width="formLabelWidth">
                    <el-upload
                      class="avatar-uploader"
                      action="https://jsonplaceholder.typicode.com/posts/"
                      :headers="headers"
                      :show-file-list="false"
                      :on-success="handleAvatarSuccess"
                      :before-upload="beforeAvatarUpload">
                      <img v-show="categoryForm.icon" :src="categoryForm.icon" class="avatar">
                      <i v-show="!categoryForm.icon" class="el-icon-plus avatar-uploader-icon"></i>
                    </el-upload>
                    <!--          action="http://192.168.1.135:7001/api/v1/common/upload"-->
                  </el-form-item>
                  <el-form-item label="备注说明" :label-width="formLabelWidth" prop="memo">
                    <el-input type="textarea" placeholder="请输入内容" v-model="categoryForm.memo" :rows="4" maxlength="200" show-word-limit>
                    </el-input>
                  </el-form-item>
                </el-form>
                <div slot="footer" class="dialog-footer">
                  <el-button @click="categoryDialogFormVisible = false">取 消</el-button>
                  <el-button type="primary" @click="submitCategoryForm">确 定</el-button>
                </div>
              </el-dialog>
            </div>
          </div>
        </div>
      </div>
      <div v-if="currentStep === 3" class="meta-setting step">
      </div>
    </div>
  </div>
</template>

<script>
import { addCategory, putCategory, delCategory, getCategoryList } from '@/api/category'
import { generateTree, generateList, isNull } from '@/libs/factory'
import GeoTree from 'src/components/geo-tree'
import { getList } from '@/api/common'
import { getClassifyList, addClassify, PutClassify } from 'src/api/classification'
import Api from 'src/api/config'
// import { dateFormart } from 'src/libs/factory'
const param = {
  keyName: 'id',
  parentName: 'parentId',
  childrenName: 'children',
  label: 'name'
}
export default {
  name: 'training',
  components: {
    GeoTree
  },
  mounted () {
    this.init()
  },
  computed: {
    containerClass: function () {
      if (this.currentStep === 1 || this.currentStep === 3) return 'small'
      else return 'normal'
    }
  },
  data () {
    return {
      // 创建样本集之前设置的检索条件
      queryFactor: false,
      showDetail: false,
      id: 0,
      category: '',
      categoryList: [],
      categoryTable: [],
      record: [],
      page: 1,
      opt: undefined,
      project: undefined,
      treeData: [],
      defaultProps: {
        children: 'children',
        label: 'label'
      },
      classDialogFormVisible: false,
      categoryDialogFormVisible: false,
      formLabelWidth: '120px',
      categoryForm: {
        name: '',
        fillColor: '',
        storkColor: '',
        icon: '',
        code: null,
        memo: ''
      },
      classForm: {
        taonomyName: '',
        taonomyType: 0,
        memo: ''
      },
      rules: {
        name: [
          { required: true, message: '分类名称不能为空!', trigger: 'blur' }
        ],
        storkColor: [
          { required: true, message: '边框颜色不能为空!', trigger: 'blur' }
        ]
      },
      formTitle: '新增分类',
      list: [],
      dragItem: null,
      dragType: 'add',
      treeNodeKey: [],
      removeDrag: undefined,
      headers: {
        Authorization: {}
      },
      // above is from geospot
      currentStep: 1,
      // 地物提取还是变化监测任务
      sessionType: '',
      rules1: {
        name: [ { required: true, message: '请输入名称', trigger: 'blur' },
          { min: 3, max: 15, message: '长度在 3 到 15 个字符', trigger: 'blur' } ],
        memo: [ { message: '请填写描述信息', trigger: 'blur' } ]
      },
      sampleSetList: [],
      addSessionParams: {
        name: '',
        memo: '',
        dataUrl: '',
        parameter: '',
        trainType: '',
        algName: '',
        algorithmId: -1,
        dataId: -1
      },
      // 被选中的分类体系的Id
      mainId: null
    }
  },
  methods: {
    exit () {
      this.$router.push({ name: 'basicSet' })
    },
    back (code) {
      if (code === 1) this.currentStep = 1
      else if (code === 2) this.showDetail = false
    },
    init () {
      this.queryFactor = this.$route.params.queryFactor
      console.log(this.queryFactor)
      this.getClassList()
    },
    /**
     * Step1 (Set)
     */
    // 设置好样本集名称和描述，准备进入设置分类体系阶段
    submitSetForm () {
      this.$refs['ruleForm'].validate((valid) => {
        if (valid) {
          this.currentStep = 2
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    /**
     *  Step2 - 选择分类体系 (Class)
     */
    // 开始新增分类体系
    handleAddClass () {
      this.classForm = {
        taonomyName: '',
        taonomyType: 0,
        memo: ''
      }
      this.formTitle = '新增分类'
      this.opt = 'add'
      this.classDialogFormVisible = true
    },
    handleEditClass (row) {
      this.classForm = row
      this.formTitle = '修改分类'
      this.opt = 'edit'
      this.classDialogFormVisible = true
    },
    // 新增或者修改分类体系
    submitClassForm () {
      let _this = this
      this.$refs['classForm'].validate((valid) => {
        if (valid) {
          let promise
          // 分类体系类别为自定义分类体系
          _this.classForm.customType = 2
          if (_this.opt === 'add') {
            promise = addClassify(_this.classForm)
          } else {
            promise = PutClassify(_this.classForm)
          }
          promise.then((response) => {
            const { data } = response
            if (data && data.code === _this.$config.statusCode) {
              console.log('in here')
              _this.getClassList()
            } else {
              _this.$notify.error({ title: '错误', message: data.message })
            }
          }).catch((response) => {
            // console.log(response)
          })
          _this.classDialogFormVisible = false
        } else {
          return false
        }
      })
    },
    // 获取分类体系列表
    getClassList () {
      const _this = this
      let query = {
        customType: 2,
        page: 0,
        size: 500
      }
      getClassifyList(query).then((response) => {
        const { data } = response
        // debugger
        if (data && data.code === 200) {
          // console.log(_this.classList)
          _this.classList = data.data.list
          this.$forceUpdate()
          // console.log(_this.classList)
        } else {
          _this.$notify.error({ title: '错误', message: data.message })
        }
      }).catch((response) => {
        // console.log(response)
      })
    },
    handleClone (row) {
      //
    },
    handleFreeze (row) {
      //
    },
    // 查看分类体系的详情
    handleDetail (row) {
      // this.$router.push({ name: 'Category', params: { id: row.id } })
      this.mainId = row.id
      this.showDetail = true
      this.getCategoryList()
    },
    /**
     * 设置分类体系的内部结构 -- 类别 (category)
     */
    // 开始新增分类类别
    handleAddCategory () {
      this.categoryForm = {
        name: '',
        fillColor: '',
        storkColor: '',
        icon: '',
        code: null,
        mainId: this.mainId,
        memo: ''
      }
      this.formTitle = '新增分类'
      this.opt = 'add'
      this.categoryDialogFormVisible = true
    },
    handleEditCategory (row) {
      let id = row.id
      const _form = this.record.find((o) => o.id === id)
      this.classForm = _form
      this.formTitle = '修改分类'
      this.opt = 'edit'
      this.categoryDialogFormVisible = true
    },
    // 新增或者修改分类详情
    submitCategoryForm () {
      let _this = this
      this.$refs['categoryForm'].validate((valid) => {
        if (valid) {
          let promise
          if (_this.opt === 'add') {
            promise = addCategory(_this.categoryForm)
          } else {
            promise = _this.putCategory(_this.categoryForm)
          }
          promise.then((response) => {
            debugger
            if (response && response.data.code === _this.$config.statusCode) {
              _this.getCategoryList()
            } else {
              _this.$notify.error({ title: '错误', message: response.message })
            }
          }).catch((response) => {
            // console.log(response)
          })
          _this.categoryDialogFormVisible = false
        } else {
          return false
        }
      })
    },
    // 获取类别列表
    getCategoryList () {
      const _this = this
      let query = {
        mainId: this.mainId
      }
      if (this.category) query.name = this.category
      getCategoryList(query).then((response) => {
        if (response && response.data.code === _this.$config.statusCode) {
          const list = response.data.data.list
          _this.record = list
          _this.categoryTable = list.map((o) => {
            return {
              id: o.id,
              label: o.name,
              parentId: o.parentId
            }
          })
          _this.categoryList = _this.categoryTable.filter((o) => {
            return isNull(o.parentId)
          })
          const tree = _this.categoryTable.filter((o) => {
            return !isNull(o.parentId)
          })
          _this.treeData = generateTree(tree, param)
          _this.treeNodeKey = []
        } else {
          _this.$notify.error({ title: '错误', message: response.message })
        }
      }).catch((response) => {
        // console.log(response)
      })
    },
    // 删除分类类别
    handleDeleteCategory (row) {
      const _this = this
      delCategory(row.id).then((response) => {
        if (response && response.data.code === _this.$config.statusCode) {
          _this.getCategoryList()
        } else {
          _this.$notify.error({ title: '错误', message: response.message })
        }
      }).catch((response) => {
        // console.log(response)
      })
    },
    handleDragOver (e) {
      e.preventDefault()
    },
    drag (e, item) {
      this.dragItem = item
      this.dragType = 'add'
    },
    handleDrop (e) {
      let { id, label } = this.dragItem
      if (this.treeNodeKey.indexOf(id) > -1) {
        return false
      }
      const node = {
        id,
        label,
        children: []
      }
      this.treeData.push(node)
      let index = this.categoryList.findIndex((o) => {
        return o.id === id
      })
      this.categoryList.splice(index, 1)
      this.treeNodeKey.push(id)
    },
    allowdrop (draggingNode, dropNode, type) {
      // console.log(draggingNode, dropNode, type)
      // if (type === 'inner' || draggingNode.data.pid !== dropNode.data.pid) {
      //   return false
      // }
      return true
    },
    nodeDragStart (node, event) {
      if (node.data.children.length !== 0) {
        this.$message.warning('该节点有子节点, 请先移动子节点！')
        return false
      }
      this.removeDrag = node
    },
    handleDropList (e) {
      if (!this.removeDrag) {
        return false
      }
      let { id, label } = this.removeDrag.data
      let index = this.treeNodeKey.findIndex((o) => {
        return o === id
      })
      this.categoryList.push({ id, label })
      this.treeNodeKey.splice(index, 1)
    },
    // 更新节点树
    save () {
      const _this = this
      const list = generateList(this.treeData, param)
      const putData = []
      for (let i = 0; i < list.length; i++) {
        const item = list[i]
        let treeNode = this.categoryTable.find((o) => {
          return o.id === item.id
        })
        if (treeNode) {
          if (treeNode.parentId !== item.parentId) {
            putData.push(list[i])
          }
        }
      }
      console.log(putData)
      putCategory({ data: putData }).then((response) => {
        if (response && response.data.code === _this.$config.statusCode) {
          _this.$notify.success({ title: '提示', message: '节点数保存成功！' })
        } else {
          _this.$notify.error({ title: '错误', message: response.message })
        }
        _this.showDetail = false
      }).catch((response) => {
        // console.log(response)
      })
    },
    handleAvatarSuccess (res, file) {
      const _this = this
      // let url = URL.createObjectURL(file.raw)
      let reader = new FileReader()
      reader.readAsDataURL(file.raw)
      reader.onload = function (e) {
        _this.classForm.icon = e.target.result
        _this.$set(_this.classForm, e.target.result, 'icon')
      }
    },
    beforeAvatarUpload (file) {
      // const isJPG = file.type === 'image/jpeg'
      const isLt2M = file.size < (50 * 1024)

      // if (!isJPG) {
      //   this.$message.error('上传头像图片只能是 JPG 格式!')
      // }
      if (!isLt2M) {
        this.$message.error('上传头像图片大小不能超过 50K!')
      }
      return isLt2M
    }
  }
}
</script>

<style lang="scss">
@import "createSet";
@import "../layout";
</style>
<style lang="css">
  .category .category-list {
    width: 600px;
    padding: 0 40px;
  }
  .category-list .td-opt {
    flex: 2
  }
  .category-list .search-tree li {
    line-height: 32px;
    height: 36px;
  }
  .category .table-panel {
    /*height: calc(100vh - 175px);*/
    background-color: #ffffff;
    padding: 15px;
    display: flex;
  }
  .flex-placeholder {
    flex: 0.5;
  }
  .classify-tree {
    flex: 1;
    border: 1px solid #eeeeee;
  }
  .el-tree {
    height: 100%;
  }
  .category .search-tree {
    /*height: calc(100vh - 210px);*/
    overflow-y: auto;
  }
  .category .center-buttom {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
