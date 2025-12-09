



# ruoyi框架低代码学习--通往视频网站的最后一舞

### 1  登录功能的开启

基础篇6 其他功能



### 2 前后端连调

基础篇15-16  比较重要



### 3 二次开发案例

重点是设计数据库表和新建文件！

基础篇17的后4分钟直接照抄



# 视频网站  是的直接开始

### 1 登录功能 

#### 1-1 用户表结构

| 字段名        | 数据类型                | 描述                                                 | 是否必填 |
| ------------- | ----------------------- | ---------------------------------------------------- | -------- |
| user_id       | INT/AUTO_INCREMENT      | 用户ID，主键，自动增长                               | 是       |
| username      | VARCHAR(255)            | 用户名                                               | 是       |
| password      | VARCHAR(255)            | 密码，应存储加密后的哈希值                           | 是       |
| email         | VARCHAR(255)            | 邮箱地址，用于登录和找回密码                         | 是       |
| role          | ENUM('admin', 'viewer') | 用户角色，区分管理员和普通用户                       | 是       |
| created_at    | TIMESTAMP               | 账户创建时间，默认为当前时间                         | 是       |
| updated_at    | TIMESTAMP               | 账户最后更新时间，默认为当前时间，每次更新时自动更新 | 是       |
| is_active     | BOOLEAN                 | 账户是否激活，用于确认邮箱等操作                     | 是       |
| reset_token   | VARCHAR(255)            | 用于重置密码的临时令牌                               | 否       |
| reset_expires | DATETIME                | 重置密码令牌的有效期                                 | 否       |

```sql
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    role ENUM('admin', 'viewer') NOT NULL DEFAULT 'viewer',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    is_active BOOLEAN NOT NULL DEFAULT FALSE,
    reset_token VARCHAR(255),
    reset_expires DATETIME
);
```



#### 1-2 后端登录校验

选择md5密码加密（仅管理员用户 其他人的密码无所谓）

interceptor拦截器实现校验

目前来看没什么难度  还不如swagger的配置难



#### 1-3 前端页面

这里先舍弃美观 仅考虑功能

##### 注意点1：反向代理 

目前有点懂了 前端要向后端发送请求就得通过反向代理来配置路径的映射

这个表示将api替换成'http://localhost:8080',

给一个实际例子：

**登录**

- 用户访问 `http://localhost:8888/login`。
- 用户提交表单，发起请求 `http://localhost:8888/api/auth/login`。
- **请求通过反向代理转发到** `http://localhost:8080/auth/login`。

这里将api开头的请求全部代理到'http://localhost:8080',   包括前面的8888前端端口 也代理到了后端的8080

- 登录成功后，用户被重定向到 `http://localhost:8888/`。

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  devServer: {
    port: 8888,
    proxy: {
      '/api': {
        target: 'http://localhost:8080',
        changeOrigin: true, // 需要改变请求头中的origin
        pathRewrite: {
        
        }
      }
    }
  },
  
  transpileDependencies: true

  
})

```



##### 注意点2： 从后端返回体中取数据  两个.data

1. *{code: 1, msg: 'success', data: {…}}*

2. 1. **code**: 1
   2. **data**: {id: 1, userName: 'cqy', ifadmin: true, token: 'eyJhbGciOiJIUzI1NiJ9.eyJlbXBJZCI6bnVsbCwiZXhwIjoxN…jE1fQ.Ezr965ZLZzemB5Y9Z5GnprMEyMR2siAhG1A5E6EohRg'}
   3. **msg**: "success"
   4. [[Prototype]]: Object

3. 

4. 一般后端返回体是上面这样   

5. 那么：**如果要取出token或者username呢？**

6. 要用两个`.data`   因为嵌套了两层 这里没注意的话会报错很多次！

7. ```ts
   response.data && response.data.code === 1 && response.data.data && response.data.data.token
   ```

8. 



##### 注意点3： 不要用ts和request!  太复杂了

你就写单个页面 所有逻辑都在login.vue中

```vue
<template>
  <div class="login-container">
    <el-card class="login-card">
      <el-form @submit.prevent="handleLogin" label-position="top">
        <div class="form-header">
          <h2>Login</h2>
        </div>
        <el-form-item label="Username">
          <el-input v-model="username" placeholder="Username" required></el-input>
        </el-form-item>
        <el-form-item label="Password">
          <el-input v-model="password" type="password" placeholder="Password" required></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" native-type="submit" class="login-button">Login</el-button>
        </el-form-item>
      </el-form>
    </el-card>
    <p v-if="errorMessage" style="display: none;">{{ errorMessage }}</p>
  </div>
</template>

<script>
import { mapState } from 'vuex'; // 导入 mapState
import { onMounted } from 'vue';
import router from '@/router';
import axios from 'axios';

export default {
  data() {
    return {
      username: '',
      password: '',
      errorMessage: '' // 添加 errorMessage
    };
  },
  computed: {
    // 映射 isLoggedIn 从 Vuex store
    isLoggedIn() {
      return this.$store.getters.isLoggedIn; // 假设 isLoggedIn 是一个 getter
    }
  },
  methods: {
    async handleLogin() {
      try {
        const response = await axios.post('/api/auth/login', {
          username: this.username,
          password: this.password,
        });

        // 登录成功后跳转到主页
        if (response.data && response.data.code === 200 && response.data.data && response.data.data.token) {
          const token = response.data.data.token;
          localStorage.setItem('token', token);
          this.$router.push('/');
          //console.log(response.data);
        } else if (response.status === 200 && response.data && response.data.code !== 200) {
        // 如果后端返回的结果不符合预期，抛出错误
        //console.error(response.data);
        throw new Error(JSON.stringify(response.data));
      } else {
        // 如果 HTTP 状态码不是 200，抛出错误
        throw new Error('未知错误');
      }
    } catch (error) {
      let errorMessage = '登录失败，请检查用户名和密码';

  if (error.response) {
    // 服务器响应了，但状态码不是2xx
    errorMessage = error.response.data.msg || '未知错误';
  } else if (error.request) {
    // 请求已发出，但没有收到响应
    errorMessage = '无法连接到服务器，请检查您的网络设置';
    console.log('请求已发出，但没有收到响应:', error.request);
  } else if (error.message) {
    // 检查 error.message 是否是 JSON 格式
    try {
      const errorData = JSON.parse(error.message);
      if (errorData && errorData.code) {
        errorMessage = errorData.msg;
      }
    } catch (e) {
      console.error('解析错误信息时发生异常:', e);
      // 继续使用默认的 errorMessage
    }
  } else {
    // 发生了一些其他错误
    errorMessage = "发生未知的错误";
  }

  this.errorMessage = errorMessage;
  alert(errorMessage);
  console.error('登录错误:', errorMessage);
    }
  }
},
  mounted() {
    // 检查是否已经登录
    if (this.isLoggedIn) {
      router.push('/');
    }
  }
}
</script>

<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f2f5; /* 背景颜色 */
  margin: 0; /* 确保没有额外的边距 */
  padding: 0; /* 确保没有额外的填充 */
}

.login-card {
  width: 500px; /* 登录卡片宽度 */
  padding: 30px;
  background-color: #ffffff; /* 卡片背景颜色 */
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
}

.form-header {
  text-align: center;
  margin-bottom: 20px;
}

.form-header h2 {
  color: #5c085a; /* 文字颜色 */
  font-size: 24px;
  margin: 0;
}

.el-form-item {
  margin-bottom: 20px;
}

.login-button {
  width: 100%;
}
</style>
```





##### 注意点4：前端throw error  无法被catch

这是因为 你的代码没有改变状态码 

本来状态码200是成功 401是失败  但是你全部返回的200  没有修改 而是通过自定义的result.code来代替了这个值

所以catch是识别不到的  必须在try里面判断





#### 1-4 新建用户

##### 注意点1：新建用户时  邮箱和用户id必须唯一 所以需要加判断

##### 注意点2： 统一将状态码封装到status.constant中

也就是result里的code 这样是为了规范





### 2 后台视频管理

#### 2-1 上传视频--页面分页开发

数据库构造  这里关键是id有个外键 所以给出代码

```sql
CREATE TABLE `videos` (
  `id` int NOT NULL AUTO_INCREMENT,
  `title` varchar(255) NOT NULL,
  `description` text,
  `created_at` datetime DEFAULT CURRENT_TIMESTAMP,
  `updated_at` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `category_id` int DEFAULT NULL,
  `views` int DEFAULT '0',
  `file_path` varchar(255) NOT NULL,
  `file_name` varchar(255) DEFAULT NULL,
  `status` enum('active','inactive') DEFAULT 'active',
  `cover_image` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `fk_category_id` (`category_id`),
  CONSTRAINT `fk_category_id` FOREIGN KEY (`category_id`) REFERENCES `categories` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```



#### 2-2 关于上传视频Controller   文件路径的选择

##### 1， 在Controller中

```Java
Map<String, String> videoResult = saveFile(videoFile, "videos", uploadDir);
Map<String, String> coverResult = saveFile(coverImage, "covers", uploadDir);
```

因此输入的路径是

`folderPath = ".\\uploadDir\\videos"`    也就是在uploadDir这个**相对路径**下又拼了两个文件夹

##### 2， 在savefile函数中

首先生成一个唯一文件名 用uuid来生成  假设是

```
2f3b4c6d.mp4
```

##### 3， 文件保存路径

```Java
Path filePath = folderPath.resolve(uniqueFilename);
```

这一步会将 `uniqueFilename` 附加到 `folderPath` 上，生成最终的文件保存路径。



因此在windows上 就是

```
.\videos_test\videos\2f3b4c6d.mp4
```





完整的代码

```Java
@PostMapping("/upload")
    @ApiOperation("视频上传功能")
    @Transactional
    public ResponseEntity<?> uploadVideo(
            @RequestParam("title") String title,
            @RequestParam("description") String description,
            @RequestParam("category") Long category,
            @RequestParam("videoFile") MultipartFile videoFile,
            @RequestParam("coverImage") MultipartFile coverImage) {


        if (uploadDir == null || uploadDir.isEmpty()) {
            throw new ResponseStatusException(HttpStatus.INTERNAL_SERVER_ERROR, "上传目录未配置");
        }
        // 保存视频文件
        try {
            Map<String, String> videoResult = saveFile(videoFile, "videos", uploadDir);
            Map<String, String> coverResult = saveFile(coverImage, "covers", uploadDir);

            String videoPath = videoResult.get("filePath");
            String coverImagePath = coverResult.get("filePath");
            String videoName = videoResult.get("uniqueFilename");

            videoService.uploadVideo(title, description, category, videoPath, coverImagePath,videoName);

            // 创建返回实体对象
            VideoVo response = VideoVo.builder()
                    .title(title)
                    .categoryId(category)
                    .filepath(videoPath)
                    .coverImage(coverImagePath)
                    .message("视频上传成功")
                    .status("success")
                    .build();

            return ResponseEntity.status(HttpStatus.CREATED).body(response);
        } catch (IOException e) {
            //这个值是500
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                    .body("文件上传失败: " + e.getMessage());
        }
    }



private Map<String, String> saveFile(MultipartFile file, String folder, String dir) throws IOException {
        if (file.isEmpty()) {
            throw new ResponseStatusException(HttpStatus.BAD_REQUEST, "文件为空");
        }

        // 确保文件夹存在
        Path folderPath = Paths.get(dir, folder);
        if (!Files.exists(folderPath)) {
            Files.createDirectories(folderPath);
        }

        // 获取文件的原始名称
        String originalFilename = file.getOriginalFilename();
        if (originalFilename == null) {
            throw new ResponseStatusException(HttpStatus.BAD_REQUEST, "文件没有名字");
        }

        // 获取文件扩展名
        String extension = originalFilename.substring(originalFilename.lastIndexOf("."));

        // 生成唯一的文件名，使用 UUID（去除编码）
        String uniqueFilename = UUID.randomUUID().
                toString().replace("-", "").substring(0, 8) + extension;

        // 生成文件保存路径
        Path filePath = folderPath.resolve(uniqueFilename);

        // 保存文件
        try (InputStream inputStream = file.getInputStream()) {
            Files.copy(inputStream, filePath, StandardCopyOption.REPLACE_EXISTING);
        }

        Map<String, String> result = new HashMap<>();
        result.put("filePath", filePath.toString());
        result.put("uniqueFilename", uniqueFilename);

        return result;
    }
```







#### 2-3 上传和删除  事务

由于上传和删除 必须同时保证对数据库内的表信息和实际视频进行操作

因此必须用@Transactional进行封装



#### 2-4 视频分类管理 前端页面大成

这里有一个完整的 分页展示+操作+查询  有需要自己去找对应的代码学习吧

![wis3-1](1_picture/wis3-1.PNG)

```vue
<template>
  <div class="category-management-container">
    <!-- 分类操作区域 -->
    <div class="button-group">
      <!-- 搜索框和按钮放在同一行 -->
      <div class="search-container">
        <el-input v-model="searchKeyword" placeholder="请输入分类名称进行搜索" prefix-icon="el-icon-search" clearable
          @input="handleSearch" class="search-input" />
        <el-button @click="handleSearch" type="primary" icon="el-icon-search" class="search-button">
          <span class="button-text">搜索</span>
        </el-button>
      </div>

      <!-- 添加视频分类按钮，靠右对齐 -->
      <el-button class="add-category-button" type="primary" @click="showAddCategoryDialog">添加视频分类</el-button>
    </div>

    <!-- 分类列表 -->
    <el-table :data="categories" style="width: 100%" header-align="center" align="center">
      <el-table-column label="分类 ID" prop="id" width="300"></el-table-column>
      <el-table-column label="分类名称" prop="name" width="650"></el-table-column>
      <el-table-column label="操作" width="400" fixed="right">
        <template #default="scope">
          <el-button @click="editCategory(scope.row)" size="mini" type="primary">编辑</el-button>
          <el-button @click="deleteCategory(scope.row)" size="mini" type="danger">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 分页组件 -->
    <el-pagination v-show="total > 0" :current-page.sync="pagination.page" :page-size.sync="pagination.limit"
      :total="total" layout="prev, pager, next, jumper" @current-change="getCategories" />

    <!-- 添加分类对话框 -->
    <el-dialog title="添加分类" v-model="addCategoryDialogVisible">
      <el-form :model="newCategory" label-width="80px">
        <el-form-item label="分类名称">
          <el-input v-model="newCategory.name" placeholder="请输入分类名称"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="addCategoryDialogVisible = false">取消</el-button>
        <el-button type="primary" @click="submitNewCategory">确定</el-button>
      </span>
    </el-dialog>

    <!-- 编辑分类对话框 -->
    <el-dialog title="修改分类" v-model="editCategoryDialogVisible">
      <el-form :model="selectedCategory" label-width="80px">
        <el-form-item label="分类名称">
          <el-input v-model="selectedCategory.name" placeholder="请输入分类名称"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="editCategoryDialogVisible = false">取消</el-button>
        <el-button type="primary" @click="updateCategory">保存</el-button>
      </span>
    </el-dialog>

    <!-- 删除分类确认对话框 -->
    <el-dialog title="确认删除" v-model="deleteCategoryDialogVisible">
      <p>您确定要删除分类 <strong>{{ selectedCategory.name }}</strong> 吗？删除该分类会将该分类下所有视频一并删除，请确保该分类为空。</p>
      <span slot="footer" class="dialog-footer">
        <el-button @click="deleteCategoryDialogVisible = false">取消</el-button>
        <el-button type="danger" @click="confirmDeleteCategory">确定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import { reactive, ref, onMounted } from "vue";
import Request from "@/utils/request";
import { ElMessage } from 'element-plus';

export default {
  setup() {
    // 响应式数据
    const categories = ref([]); // 分类列表
    const total = ref(0); // 分类总数
    const pagination = reactive({ page: 1, limit: 10 }); // 分页
    const searchKeyword = ref(''); // 搜索关键字
    const addCategoryDialogVisible = ref(false); // 控制添加分类对话框显示
    const editCategoryDialogVisible = ref(false); // 控制编辑分类对话框显示
    const deleteCategoryDialogVisible = ref(false); // 控制删除分类对话框显示
    const newCategory = reactive({ name: '' }); // 新建分类的数据模型
    const selectedCategory = reactive({ id: '', name: '' }); // 当前选中的分类数据

    // 获取分类数据（可根据搜索条件）
    const getCategories = async () => {
      try {
        let url = 'admin/category'; // 默认查询所有分类
        const params = {
          page: pagination.page,
          limit: pagination.limit,
        };

        // 如果有搜索关键字，使用模糊查询接口
        if (searchKeyword.value.trim()) {
          url = 'admin/category/search'; // 假设搜索接口的路径是 admin/category/search
          params.keyword = searchKeyword.value; // 加入搜索关键字
        }

        const response = await Request.get(url, { params });

        // 确保接口返回的数据是我们需要的结构
        if (response.data && response.data.categories) {
          categories.value = response.data.categories;
          total.value = response.data.total;
        } else {
          console.error("返回的数据格式不符合预期", response.data);
        }
      } catch (error) {
        console.error("获取分类列表失败", error);
      }
    };

    // 显示添加分类对话框
    const showAddCategoryDialog = () => {
      newCategory.name = ''; // 清空表单
      addCategoryDialogVisible.value = true;
    };

    // 提交新增分类
    const submitNewCategory = async () => {
      try {
        await Request.post('admin/category', newCategory); // 发起添加请求
        addCategoryDialogVisible.value = false; // 关闭对话框
        getCategories(); // 刷新分类列表
      } catch (error) {
        console.error("分类添加失败", error);
      }
    };

    // 显示编辑分类对话框
    const editCategory = (category) => {
      if (!category) return;
      selectedCategory.id = category.id; // 传递分类ID
      selectedCategory.name = category.name; // 传递分类名称
      editCategoryDialogVisible.value = true;
    };

    // 提交修改分类
    const updateCategory = async () => {
      try {
        await Request.put(`admin/category/${selectedCategory.id}`, selectedCategory); // 发起更新请求
        editCategoryDialogVisible.value = false; // 关闭对话框
        getCategories(); // 刷新分类列表
      } catch (error) {
        console.error("分类更新失败", error);
      }
    };

    // 显示删除分类对话框
    const deleteCategory = (category) => {
      if (!category) return;
      selectedCategory.id = category.id; // 传递分类ID
      selectedCategory.name = category.name; // 传递分类名称
      deleteCategoryDialogVisible.value = true;
    };

    // 确认删除分类
    const confirmDeleteCategory = async () => {
      try {
        await Request.delete(`admin/category/${selectedCategory.id}`); // 发起删除请求
        deleteCategoryDialogVisible.value = false; // 关闭对话框
        getCategories(); // 刷新分类列表
      } catch (error) {
        console.error("分类删除失败", error);
      }
    };

    // 处理搜索框输入事件
    const handleSearch = () => {
      pagination.page = 1; // 每次搜索时，重置到第一页
      getCategories(); // 输入时进行搜索
    };

    // 页面加载时获取所有分类
    onMounted(() => {
      getCategories(); // 初次加载时获取分类数据
    });

    // 返回数据和方法
    return {
      categories,
      total,
      pagination,
      searchKeyword,
      addCategoryDialogVisible,
      editCategoryDialogVisible,
      deleteCategoryDialogVisible,
      newCategory,
      selectedCategory,
      getCategories,
      showAddCategoryDialog,
      submitNewCategory,
      editCategory,
      updateCategory,
      deleteCategory,
      confirmDeleteCategory,
      handleSearch,
    };
  }
};
</script>

<style scoped>
.category-management-container {
  padding: 20px;
  background-color: #f5f5f5;
}

.button-group {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.search-container {
  display: flex;
  align-items: center;
  width: 400px;
  /* 搜索框和按钮的总宽度 */
  margin: 0 auto;
  /* 搜索框和按钮居中 */
}

.search-input {
  width: 100%;
  /* 搜索框占满剩余空间 */
  margin-right: 40px;
  /* 搜索框与搜索按钮之间的间距 */
}

.add-category-button {
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 5px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  background: linear-gradient(135deg, #42b983, #2c8f65);
  color: white;
  border: none;
  transition: background-color 0.3s ease;
}

.add-category-button:hover {
  background-color: #36a058;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.search-button {
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 5px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  background: linear-gradient(135deg, #42b983, #2c8f65);
  color: white;
  border: none;
  transition: background-color 0.3s ease;
}

.search-button:hover {
  background-color: #36a058;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.search-button .button-text {
  margin-left: -20px;
  /* 调整图标和文本之间的间距 */
}

.dialog-footer {
  text-align: right;
}
</style>
```



#### 2-5 用户管理页面

这里是仿照上面的视频分类管理做的 基本没有太大困难

```vue
<template>
    <div class="user-management-container">
        <!-- 操作区域 -->
        <div class="operation-area">
            <!-- 搜索框和按钮放在同一行 -->
            <div class="search-container">
                <el-input v-model="searchKeyword" placeholder="请输入工号进行搜索" prefix-icon="el-icon-search" clearable
                    @input="handleSearch" class="search-input" />
                <el-button @click="handleSearch" type="primary" icon="el-icon-search" class="search-button">
                    <span class="button-text">搜索</span>
                </el-button>
            </div>

            <!-- 添加用户按钮，靠右对齐 -->
            <el-button class="add-user-button" type="primary" @click="showAddUserDialog">添加用户</el-button>
        </div>

        <!-- 用户列表 -->
        <el-table :data="users" style="width: 100%" header-align="center" align="center">
            <el-table-column label="用户 ID" prop="userId" width="100"></el-table-column>
            <el-table-column label="工号" prop="username" width="150"></el-table-column>
            <el-table-column label="姓名" prop="realname" width="150"></el-table-column>
            <el-table-column label="邮箱" prop="email" width="250"></el-table-column>
            <el-table-column label="密码" prop="password" width="200"></el-table-column>
            <el-table-column label="创建时间" prop="createdAtStr" width="200"></el-table-column>
            <el-table-column label="更新时间" prop="updatedAtStr" width="200"></el-table-column>
            <el-table-column label="操作" width="200" fixed="right">
                <template #default="scope">
                    <el-button @click="editUser(scope.row)" size="mini" type="primary">编辑</el-button>
                    <el-button @click="deleteUser(scope.row)" size="mini" type="danger">删除</el-button>
                </template>
            </el-table-column>
        </el-table>

        <!-- 分页组件 -->
        <el-pagination v-show="total > 0" :current-page.sync="pagination.page" :page-size.sync="pagination.limit"
            :total="total" layout="prev, pager, next, jumper" @current-change="getUsers" />

       <!-- 添加用户对话框 -->
       <el-dialog title="添加用户" v-model="addUserDialogVisible">
            <el-form :model="newUser" label-width="80px">
                <el-form-item label="工号">
                    <el-input v-model="newUser.employeeNumber" placeholder="请输入工号"></el-input>
                </el-form-item>
                <el-form-item label="姓名">
                    <el-input v-model="newUser.name" placeholder="请输入姓名"></el-input>
                </el-form-item>
                <el-form-item label="邮箱">
                    <el-input v-model="newUser.email" placeholder="请输入邮箱"></el-input>
                </el-form-item>
                <el-form-item label="密码">
                    <el-input v-model="newUser.password" placeholder="请输入密码" type="password"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="addUserDialogVisible = false">取消</el-button>
                <el-button type="primary" @click="submitNewUser">确定</el-button>
            </span>
        </el-dialog>

        <!-- 编辑用户对话框 -->
        <el-dialog title="修改用户" v-model="editUserDialogVisible">
            <el-form :model="selectedUser" label-width="80px">
                <el-form-item label="工号">
                    <el-input v-model="selectedUser.employeeNumber" placeholder="请输入工号"></el-input>
                </el-form-item>
                <el-form-item label="姓名">
                    <el-input v-model="selectedUser.name" placeholder="请输入姓名"></el-input>
                </el-form-item>
                <el-form-item label="邮箱">
                    <el-input v-model="selectedUser.email" placeholder="请输入邮箱"></el-input>
                </el-form-item>
                <el-form-item label="密码">
                    <el-input v-model="selectedUser.password" placeholder="请输入密码" type="password"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="editUserDialogVisible = false">取消</el-button>
                <el-button type="primary" @click="updateUser">保存</el-button>
            </span>
        </el-dialog>

        <!-- 删除用户确认对话框 -->
        <el-dialog title="确认删除" v-model="deleteUserDialogVisible">
            <p>您确定要删除用户 <strong>{{ selectedUser.name }}</strong> 吗？</p>
            <span slot="footer" class="dialog-footer">
                <el-button @click="deleteUserDialogVisible = false">取消</el-button>
                <el-button type="danger" @click="confirmDeleteUser">确定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
import { reactive, ref, onMounted } from "vue";
import Request from "@/utils/request";
import { ElMessage } from 'element-plus';

export default {
    setup() {
        // 响应式数据
        const users = ref([]); // 用户列表
        const total = ref(0); // 用户总数
        const pagination = reactive({ page: 1, limit: 10 }); // 分页
        const searchKeyword = ref(''); // 搜索关键字
        const addUserDialogVisible = ref(false); // 控制添加用户对话框显示
        const editUserDialogVisible = ref(false); // 控制编辑用户对话框显示
        const deleteUserDialogVisible = ref(false); // 控制删除用户对话框显示
        const newUser = reactive({ employeeNumber: '', name: '', email: '', password: '' }); // 新建用户的数据模型
        const selectedUser = reactive({ id: '', employeeNumber: '', name: '', email: '', password: '' }); // 当前选中的用户数据

        // 获取用户数据（可根据搜索条件）
        const getUsers = async () => {
            try {
                let url = 'admin/users'; // 默认查询所有用户
                const params = {
                    page: pagination.page,
                    limit: pagination.limit,
                };

                // 如果有搜索关键字，使用模糊查询接口
                if (searchKeyword.value.trim()) {
                    url = 'admin/users/search'; // 假设搜索接口的路径是 admin/users/search
                    params.keyword = searchKeyword.value; // 加入搜索关键字
                }

                const response = await Request.get(url, { params });
                console.log(response.data);  // 打印接口返回的数据

                // 确保接口返回的数据是我们需要的结构
                if (response.data && response.data.users) {
                    users.value = response.data.users;
                    total.value = response.data.total;
                } else {
                    console.error("返回的数据格式不符合预期", response.data);
                }
            } catch (error) {
                console.error("获取用户列表失败", error);
            }
        };

        // 显示添加用户对话框
        const showAddUserDialog = () => {
            newUser.employeeNumber = ''; // 清空表单
            newUser.name = '';
            newUser.email = '';
            newUser.password = '';
            addUserDialogVisible.value = true;
        };

        // 提交新增用户
        const submitNewUser = async () => {
            try {
                await Request.post('admin/users', newUser); // 发起添加请求
                addUserDialogVisible.value = false; // 关闭对话框
                getUsers(); // 刷新用户列表
            } catch (error) {
                console.error("用户添加失败", error);
            }
        };

        // 显示编辑用户对话框
        const editUser = (user) => {
            if (!user) return;
            selectedUser.id = user.userId; // 传递用户ID
            selectedUser.employeeNumber = user.username; // 传递工号
            selectedUser.name = user.realname; // 传递姓名
            selectedUser.email = user.email; // 传递邮箱
            selectedUser.password = user.password; // 传递密码
            editUserDialogVisible.value = true;
        };

        // 提交修改用户
        const updateUser = async () => {
            try {
                await Request.put(`admin/users/${selectedUser.id}`, selectedUser); // 发起更新请求
                editUserDialogVisible.value = false; // 关闭对话框
                getUsers(); // 刷新用户列表
            } catch (error) {
                console.error("用户更新失败", error);
            }
        };

        // 显示删除用户对话框
        const deleteUser = (user) => {
            if (!user) return;
            selectedUser.id = user.id; // 传递用户ID
            selectedUser.name = user.name; // 传递姓名
            deleteUserDialogVisible.value = true;
        };

        // 确认删除用户
        const confirmDeleteUser = async () => {
            try {
                await Request.delete(`admin/users/${selectedUser.id}`); // 发起删除请求
                deleteUserDialogVisible.value = false; // 关闭对话框
                getUsers(); // 刷新用户列表
            } catch (error) {
                console.error("用户删除失败", error);
            }
        };

        // 处理搜索框输入事件
        const handleSearch = () => {
            pagination.page = 1; // 每次搜索时，重置到第一页
            getUsers(); // 输入时进行搜索
        };

        // 页面加载时获取所有用户
        onMounted(() => {
            getUsers(); // 初次加载时获取用户数据
        });

        // 返回数据和方法
        return {
            users,
            total,
            pagination,
            searchKeyword,
            addUserDialogVisible,
            editUserDialogVisible,
            deleteUserDialogVisible,
            newUser,
            selectedUser,
            getUsers,
            showAddUserDialog,
            submitNewUser,
            editUser,
            updateUser,
            deleteUser,
            confirmDeleteUser,
            handleSearch,
        };
    }
};
</script>

<style scoped>
.user-management-container {
    padding: 20px;
    background-color: #f5f5f5;
    width: 1500px;
}

.operation-area {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}

.search-container {
    display: flex;
    align-items: center;
    width: 400px;
    /* 搜索框和按钮的总宽度 */
    margin: 0 auto;
    /* 搜索框和按钮居中 */
}

.search-input {
    width: 100%;
    /* 搜索框占满剩余空间 */
    margin-right: 40px;
    /* 搜索框与搜索按钮之间的间距 */
}

.add-user-button {
    padding: 10px 20px;
    font-size: 16px;
    border-radius: 5px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
    background: linear-gradient(135deg, #42b983, #2c8f65);
    color: white;
    border: none;
    transition: background-color 0.3s ease;
}

.add-user-button:hover {
    background-color: #36a058;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.search-button {
    padding: 10px 20px;
    font-size: 16px;
    border-radius: 5px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
    background: linear-gradient(135deg, #42b983, #2c8f65);
    color: white;
    border: none;
    transition: background-color 0.3s ease;
}

.search-button:hover {
    background-color: #36a058;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.search-button .button-text {
    margin-left: -20px;
    /* 调整图标和文本之间的间距 */
}

.dialog-footer {
    text-align: right;
    width: 100px;
}
</style>
```



# 2-XX 目前为止值得注意的点

#### 1，前后端传数据

前端是能接受到后端 传过来的Result里的类的  所以在接受的时候要和后端的User字段保持一致！！！

以这里为例   `selectedUser是前端的数据结构`  `user是后端传过来的`



那么selectedUser的变量你随便定义名称都行    **`但是user的必须和后端entity实体类的字段名保持一致`**

```js
// 显示编辑用户对话框
        const editUser = (user) => {
            if (!user) return;
            selectedUser.id = user.userId; // 传递用户ID
            selectedUser.employeeNumber = user.username; // 传递工号
            selectedUser.name = user.realname; // 传递姓名
            selectedUser.email = user.email; // 传递邮箱
            selectedUser.password = user.password; // 传递密码
            editUserDialogVisible.value = true;
        };

```









#### 2， 大部分后端测试的问号 都是sql字段没对应上

你这个项目的数据库和实体类的字段名确实比较抽象，多注意一下对应就行





#### 3， xml文件无法识别

这里很搞，在后端resource中  mapper.xml路径也必须和main.java中的一致

具体来说  就是**resource/com/example/wisdrivideo/Mapper/UserMapper.xml**

之前都是直接放在resource下面 所以无法识别 太蠢了





<div class="middle-section" :style="{ marginLeft: `${leftMargin}px` }">
          <h1>中冶南方BIM视频学习网站</h1>
        </div>



<div class="middle-section" :style="{ marginLeft: `${leftMargin}px` }">
          <h1>中冶南方BIM视频学习网站</h1>
        </div>











### 日志管理：



# 测试：





后续开发功能：

**1，视频封面上传功能：取消  不然后续图片太多了**

**2，视频分类创建的同时 新建文件夹----后面的修改和删除也得改  md**

3，**上传视频的时候 根据分类来上传视频  路径必须匹配 不然全放在一起**

4，视频文件的命名  尽量符合规范 而不是直接乱码--这个无所谓了  周五改





# 部署:

#### 1，跨域问题--nginx

在开发环境下，跨域问题是通过前端的vue_config.js来解决的   但是这一版针对于**开发环境**

后续部署之后，GPT建议用nginx 但是这个文件他说也可以用

**综合解决：**

算了 到时候学nginx吧  应该不难 gpt非常不推荐用cli  而且也需要重新配置



#### 2， 静态资源路径

暂时是在yml文件中通过路径 但是还没有调通

搞定了 问题是后端有一个重复的映射类 

然后这里前端又有问题 直接把路径写死吧 太麻烦了 到时候部署多修改一点



#### 3， 数据库的配置

yml和具体的填写内容文件中  更改即可



#### 4，上传文件的目录

在两个yml中都修改uploaddir







#### 5，前端展示效果修改

1，标题字体

displayCategory

第13行 "middle-section-h1"

```vue
.middle-section h1 {
  font-size: 2rem; 
  font-weight: 700; 
  color: #000307; 
  margin: 0; 
  text-align: center; 
}
```

主页和视频详情页都是如此





2，页面布局（一行三个图片种类卡，但是左右空出白色 只有中间有）

同样 displaycategory   在左右流出多少空白那里修改参数即可

```vue
.category-container {
  display: flex;
  flex-wrap: wrap;          /* 支持换行 */
  justify-content: flex-start;  /* 每行的卡片从左对齐 */
  padding: 0 10vw; /* 左右留出 10% 的空白区域 */
  gap: 20px;  /* 控制卡片之间的间距 */
  margin-top: 20px; /* 给上面加一点间距 */
}

.category-card {
  width: calc(33.33% - 20px); /* 每个卡片占 1/3 宽度减去间距 */
  box-sizing: border-box; /* 保证宽度计算时包含 padding 和 margin */
  margin: 10px 0;  /* 控制上下间距 */
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  text-align: center;
  transition: all 0.3s ease-in-out;
}
```







3，等待杜工的前端直接ui图 进行修改

















## 前端效果修改（全部解决）

##### 1，页面布局

`1 横栏和视频分类卡片的位置`  

横栏位置已完成 明天去解决颜色和字体



  

`2 视频播放页中目录置于右侧`

##### 2，颜色

`1 横栏` 

`2 背景和按钮的颜色`   

`3 视频播放页正在播放的字体`

##### 3，字体

`1 首页标题  以及下标拼音`

`2 视频分类卡片字体` 

`3 视频播放页的目录字体`

##### 4，图标  已完成

`公司Logo替换`  （视频分类图标上线后再修改）  





# 测试 2.24-2.25

#### 1 后端管理系统--已完成

##### 1  用户管理

用户的搜索 编辑和删除 已完成

##### 2 管理员管理

管理员的新建 编辑和删除 已完成

##### 3 视频分类管理

视频分类图片的修改--杜景阳已完成

视频分类的新建，删除和搜索   已完成

##### 4 视频管理

新视频的创建和编辑 删除  已完成测试

视频的查询-正常





# 部署 2.25-？

### 1，Java后端部署 

#### 0. 在服务器上安装Java和数据库的环境

这个到时候再说

#### 1，打包jar包+命令行



```
java -jar D:\JavaPractice\wisdriVideo\target\wisdriVideo.jar --spring.profiles.active=prod

```





#### 2, 静态资源暴露---服务器修改

需要在后端的webmvccongifuration中 添加静态路径

最后两行暴露静态资源让前端访问    `到时候部署服务器记得修改`  `两个D开头的路径改成服务器路径`

```Java
 @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        log.info("开始设置静态资源映射");
        registry.addResourceHandler("/doc.html")
                .addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**")
                .addResourceLocations("classpath:/META-INF/resources/webjars/");
        registry.addResourceHandler("/videos_test/**")
                .addResourceLocations("file:D:/JavaPractice/wisdriVideo/videos_test/");
        registry.addResourceHandler("/save_video/**")
                .addResourceLocations("file:D:/JavaPractice/wisdriVideo/save_video/");
    }
```



#### 3，保存路径优化---服务器修改

这里重新部署的时候，相对路径有问题，所以加一个拼接的绝对路径

部署服务器的时候改成服务器的实际路径



2.26后续-还是选择相对路径，绝对路径在文件读取上有大问题

`相对路径需要克服的是--重新部署在其他目录下，之前的视频数据无法访问的问题`

问题不大 大不了重新上传或者复制文件夹吧





#### 4，配置文件-application-prod以及数据库链接等

配置文件的路径啊，数据库啊 等等都得重新设置一遍 

目前其他的是没问题了





### 2， 前端vue部署  ----nginx

#### 0. 服务器上安装node和npm和nginx

**nvm** 用来管理 **Node.js** 的版本。

**npm** 用来管理项目中的 **JavaScript 库** 和依赖。





#### 1， num run build 的bug

版本问题   换了这个版本可以解决

1. 卸载当前版本的 `@types/lodash`：

   bash

   复制

   ```
   npm uninstall @types/lodash
   ```

2. 安装一个较旧的版本（例如 4.14.182）：

   bash

   复制

   ```
   npm install @types/lodash@4.14.182 --save-dev
   ```

3. 重新编译项目：

   bash

   复制

   ```
   npm run build
   ```





#### 2，nginx问题

检查配置文件

```
nginx -t
```

重新加载nginx配置

```
nginx -s reload

```

确认nginx是否运行 以及停止

```
tasklist | findstr nginx

nginx -s stop
```



##### 1，静态资源配置

`注意这里的0.0.0.0是表示可以通过局域网访问`

```
 server {
        listen       80;
        server_name  0.0.0.0;
```



`然后这里相对路径有问题，到时候改绝对路径`

```
 location / {
            root   D:/softwareFile/nginx1.22.0/nginx-1.22.0/html/dist;
            index  index.html index.htm;
        }
```



```conf

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  0.0.0.0;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   D:/softwareFile/nginx1.22.0/nginx-1.22.0/html/dist;
            index  index.html index.htm;
        }

         # 代理前端 API 请求
        location /api {
            proxy_pass http://192.168.205.234:8080;  # 后端 API 地址
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }

        # 如果有需要可以加入静态资源映射
        # 例如，映射 save_video 文件夹
    

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}

```

#### 3，打包后的index.html

```html
<script defer="defer" src="/js/chunk-vendors.28f17d5f.js"></script>
        <script defer="defer" src="/js/app.b9937720.js"></script>
        <link href="/css/chunk-vendors.ddc93c7c.css" rel="stylesheet">
        <link href="/css/app.26d51104.css" rel="stylesheet">
        
```

这里的四个路径前面没有

```
'/'
```

`需要手动添加！`

或者将vue.config里改成： `publicPath: '/',`  

`注意这里必须没有那个点.`

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  publicPath: '/',
```



```
<!doctype html>
<html lang="">
    <head><meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width,initial-scale=1">
        <link rel="icon" href="favicon.ico">
        <title>wisdrivideo-vue</title>
        <script defer="defer" src="/js/chunk-vendors.28f17d5f.js"></script>
        <script defer="defer" src="/js/app.b9937720.js"></script>
        <link href="/css/chunk-vendors.ddc93c7c.css" rel="stylesheet">
        <link href="/css/app.26d51104.css" rel="stylesheet">
    </head>
    <body><noscript>
        <strong>We're sorry but wisdrivideo-vue doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app">

    </div>
</body>
</html>
```

#### 4，Localhost访问到文件夹而不是页面

这个问题很抽象  

要避免这个问题，遵循以下步骤：

（1）在vue.config里    

```js
publicPath: './',    //这一行必须是这样！！！ 那个.一去掉就报错  巨离谱
```

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  publicPath: './',    //这一行必须是这样！！！ 那个.一去掉就报错  巨离谱
  outputDir: 'dist', // 确保输出目录正确
  transpileDependencies: true,
  devServer: {
    port: 8888,
    proxy: {
      '/api': {
        target: 'http://192.168.205.234:8080',
        changeOrigin: true, // 需要改变请求头中的origin
        pathRewrite: {
          '^/videos_test': '',
          '^/auth': '/auth',
        }
      }
    }
  },
  
  transpileDependencies: true

  
})

```

(2) 然后部署了之后  

必须手动去修改index.html

`文件名不用管，会变的，但是路径错一个都不行`

就是`只能在下面的四个路径前面加一个左斜杠，不能带那个相对路径的点,`     ---------------`不能是./`

```
<link rel="icon" href="favicon.ico">
        <title>wisdrivideo-vue</title>
        <script defer="defer" src="/js/chunk-vendors.28f17d5f.js"></script>
        <script defer="defer" src="/js/app.b9937720.js"></script>
        <link href="/css/chunk-vendors.ddc93c7c.css" rel="stylesheet">
        <link href="/css/app.26d51104.css" rel="stylesheet">
```





#### 5. 下面弹出4个404

```
GET http://192.168.205.234/css/app.335a0ad9.css net::ERR_ABORTED 404 (Not Found)
192.168.205.234/:1 
            
            
            GET http://192.168.205.234/css/chunk-vendors.ddc93c7c.css net::ERR_ABORTED 404 (Not Found)
192.168.205.234/:1 
            
            
            GET http://192.168.205.234/js/chunk-vendors.28f17d5f.js net::ERR_ABORTED 404 (Not Found)
192.168.205.234/:1 
            
            
            GET http://192.168.205.234/js/app.576b32c7.js net::ERR_ABORTED 404 (Not Found)
```

解决方法：

在nginx配置文件中：不能用alias 就得用root





#### 6. 前端图片资源无法访问

好解决 在nginx配置文件中 添加额外的路径映射即可

```
location /fy3adminfy5/img{
            alias D:/softwareFile/nginx1220/nginx/html/dist/img;
        }
```

以这个为例子，就把这种直接映射到具体文件夹即可



#### 7， 后端图片（封面图）无法加载且没有发起http请求

🚨 **问题：**

1. `..\` 和 `\` 是 **Windows 风格的路径分隔符**，但浏览器只接受 `/`
2. Vue **可能从后端获取了错误的路径**
3. 浏览器 **不认 `..\`**，所以不会发起请求，导致 `F12 → Network` 没有任何请求





`终于解决了！！！！！`



总结：文件路径问题  需要改写vue后端的路径获取代码

浏览器 **不认 `..\`**，所以不会发起请求，导致 `F12 → Network` 没有任何请求



所以 需要在获取路径之后 手动修改改成格式化的标准代码





#### 8， 上传视频失败 http413说文件过大

nginx需要重新定义上传的文件大小限制  知道原因就好改了  在nginx配置文件中添加即可

#### 9，nginx+jar包后测试

已完成 没问题



#### 10，根据同事意见修改

1，前端登录页面bug 输入错误的账户提示信息过多   张燎原

2，网页token过期时间过长  刷新页面后应该直接返回登录界面    荣骁

3，microstation的视频有问题  我自己都打不开 尽量不要用mov格式的



## last--部署到Linux服务器

#### 1，复制前后端项目✅

先复制一个 省的把代码改乱了  很重要



前后端项目均放在这个目录下：

```
D:\JavaPractice\wisdrivideoLinux
```



#### 2，数据库的复制✅

这个暂时看不懂 略过



#### 3，数据库配置文件✅

后端的application中  自行修改成linux的即可

`这里是写了一大堆  直接在后端的production中一一修改  要细心`

#### 4，修改后端jar运行环境✅

**确保 `application.yml`** 适配 Linux，主要修改：

**文件路径**：Windows 是 `D:\path\to\files`，Linux 改为 `/home/your_project/files/`

这个不用说了  直接改配置文件

##### 1，静态资源✅

webconfiguration中：修改这里的路径适配linux路径

```
    registry.addResourceHandler("/videos_test/**")
                .addResourceLocations("file:D:/JavaPractice/wisdriVideo/videos_test/");
        registry.addResourceHandler("/save_video/**")
                .addResourceLocations("file:D:/JavaPractice/wisdriVideo/save_video/");
```



##### 2，日志的路径

这个是单独的配置文件  log.xml

里面也需要修改路径  最好是linux的绝对路径

在target里面自己拉一个配置文件夹log

##### 3，再强调一次  检查数据库的各种路径

#### 后端文件夹布局

在wisdriVideo文件夹下：

target----打包后的

​		里面还得新建一个log

save_video----用来存储图片和视频本身



```
java -Xms8096m -Xmx12144m -jar E:\wisdriVideo\target\wisdriVideo.jar --spring.profiles.active=prod
```





#### 5，数据库结构的复制✅

| **步骤**                     | **命令**                                                     |
| ---------------------------- | ------------------------------------------------------------ |
| **1. 导出数据库结构**        | `mysqldump -u root -p --no-data --databases your_database > your_database_schema.sql` |
| **2. 传输到 Linux**          | `scp your_database_schema.sql username@your-linux-ip:/home/your-user/` |
| **3. 连接 Linux MySQL**      | `mysql -u root -p`                                           |
| **4. 创建数据库**            | `CREATE DATABASE your_database DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;` |
| **5. 导入 SQL**              | `mysql -u root -p your_database < /home/your-user/your_database_schema.sql` |
| **6. 查看数据库**            | `USE your_database; SHOW TABLES;`                            |
| **7. 修改 Spring Boot 连接** | **修改 `application.yml`**                                   |
| **8. 重启 Spring Boot**      | `nohup java -jar your_project.jar > logs/app.log 2>&1 &`     |

后面再详细对这个步骤做总结即可 现在先有个大概思路







#### 6，前端配置

##### 1， nginx配置文件修改

感觉这个没啥可说的  挨个改吧 给出一个范例

```ini
server {
    listen 80;
    server_name your-linux-ip;  # 这里写你的 Linux 服务器 IP 或域名

    root /usr/share/nginx/html/dist;  # Vue 前端部署目录
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /api/ {  # 代理后端 API 请求
        proxy_pass http://127.0.0.1:8080/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location /save_video/ {  # 代理视频封面图片等静态资源
        alias /home/wisdri/save_video/;
        autoindex off;
    }

    location /static/ {  # Vue 静态资源
        root /usr/share/nginx/html/dist/;
    }

    client_max_body_size 2048M;  # 允许上传 2GB
}

```

📌 **关键修改点**

- `server_name your-linux-ip;` 👉 你的 **Linux 服务器 IP 或域名**
- `root /usr/share/nginx/html/dist;` 👉 **Vue 前端代码目录**
- `proxy_pass http://127.0.0.1:8080/;` 👉 **后端 API 代理**
- `alias /home/wisdri/save_video/;` 👉 **静态资源路径**
- `client_max_body_size 2048M;` 👉 允许大文件上传





##### 2， Vue配置  vue.config.js

**确保 `publicPath` 适用于 Linux**

```js
module.exports = {
  publicPath: "./", // 适用于相对路径
  outputDir: "dist",
  assetsDir: "static",
  productionSourceMap: false
};
```

这里实在是不敢改



这里的路径根据自己的经验去修改吧 相信自己



##### 3，修改环境变量.env.production

**修改 `VUE_APP_API_URL`** 在 `.env.production` 里：

```ini
VUE_APP_API_URL=http://your-linux-ip:8080
```

📌 **关键**

- 这个 URL **必须是 Linux 服务器上运行的后端地址**

我应该是没有nginx代理路径的  所以这里直接改服务器的ip即可

#### **前端总结**

| **步骤**                      | **操作**                                                     |
| ----------------------------- | ------------------------------------------------------------ |
| **1. 修改 Nginx 配置**        | `/etc/nginx/conf.d/default.conf`                             |
| **2. 修改 `vue.config.js`**   | `publicPath: "./"`                                           |
| **3. 修改 `.env.production`** | `VUE_APP_API_URL=http://your-linux-ip/api`                   |
| **4. 运行 `npm run build`**   | 生成 `dist/`                                                 |
| **5. 传输 `dist/` 到 Linux**  | `scp -r dist/ your-user@your-linux-ip:/usr/share/nginx/html/` |
| **6. 设置权限**               | `sudo chmod -R 755 /usr/share/nginx/html/`                   |
| **7. 重启 Nginx**             | `sudo nginx -s reload`                                       |
| **8. 测试前端是否能访问**     | `curl http://your-linux-ip`                                  |







**企业微信消息：**

各位同事，目前视频网站已开发完成进入测试阶段，请各位有录制新员工BIM培训视频需要上传或者有兴趣的同事共同测试一下。麻烦大家在使用过程中关注视频上传、播放、分类管理等功能是否正常，如有任何问题，欢迎及时反馈，谢谢。

用户账户和密码：在登录页面自行注册。

管理员测试账号：**每个同事的工号**
初始密码：**1234**

管理员账户的初始密码和其他信息可以在登录管理系统后自行修改。



由于目前系统仍部署在本机，主要用于功能测试，因此仅测试功能即可，无需上传过多视频。待本次测试确认无问题，正式部署至服务器后，再进行全部视频的上传。







## 部署项目--Linux放弃，在李韬windows虚拟机上部署

远程服务器的信息（非虚拟机）：192.168.145.50:7771





虚拟机信息

192.168.145.52:3389
VaultTestServer20250102.wisdri.com
当前远程桌面端口3389



#### 1，从本机直接远程到虚拟机✅

`核心问题：3389端口号被封了`

修改到3394

192.168.145.52:3394
VaultTestServer20250102.wisdri.com
当前远程桌面端口3394



##### **你的同事是如何推断 3389 被封的？**

##### **🔹 1️⃣ 现象分析**

- **你的物理机（本机） → 无法远程连接虚拟机** ❌
- **安装虚拟机的远程电脑 → 可以远程连接虚拟机** ✅

**推测：**

- 你的 **虚拟机 RDP 功能是正常的**，否则 **连远程电脑本身也无法连接**。
- 你的 **物理机和虚拟机的网络有问题**，但 **ping 是通的**，说明 **网络本身是连通的**。
- **防火墙已关闭**，排除了 **防火墙拦截的问题**。

------

##### **🔹 2️⃣ 他可能的判断思路**

| **可能的原因**               | **物理机（本机） → 远程虚拟机** | **远程电脑 → 远程虚拟机** | **符合情况？** |
| ---------------------------- | ------------------------------- | ------------------------- | -------------- |
| **虚拟机未启用 RDP**         | ❌ 无法连接                      | ❌ 无法连接                | ❌ 不符合       |
| **虚拟机 RDP 端口监听失败**  | ❌ 无法连接                      | ❌ 无法连接                | ❌ 不符合       |
| **物理机到虚拟机的网络问题** | ❌ 无法连接                      | ✅ 可以连接                | ✅ 符合         |
| **RDP 3389 端口被封**        | ❌ 无法连接                      | ✅ 可以连接                | ✅ 符合         |

由于 **只有你物理机连不上，而远程电脑可以连上**，这表明 **虚拟机的 RDP 本身是正常的**，但是**你的物理机访问 3389 端口被限制了**。这符合 **端口封禁** 的特征。

------

##### **🔹 3️⃣ 他的推理核心：端口是否在不同环境下可用**

你的同事应该是**从端口的可用性入手**：

1. **3389 端口在同一台局域网内的远程电脑上可以使用**，说明虚拟机 **RDP 端口是正常监听的**。
2. **3389 端口在你的物理机上不可用**，但是 **ping 通 IP**，说明 **网络本身通畅，问题可能出在端口封禁上**。
3. **换端口后（比如 3390），你的物理机就可以连接了**，进一步证明 **3389 端口被封，而不是 RDP 本身的问题**。

#### 2， 后端环境安装✅

这里首先查看了本机的Java环境以及项目jar包的环境

本机Java11   但是项目是1.8    

考虑过后决定虚拟机和本机保持一致   因为如果本机版本高能运行 虚拟机也没问题



Java 11.0.15 本地路径如下

`D:\BaiduNetdiskDownload\1____Javaweb黑马资料\day04-Maven-SpringBootWeb入门\day04-Maven-SpringBootWeb入门\资料\Maven\02. maven安装`



已在虚拟机完成安装



#### 3, mysql数据库和可视化工具安装✅

同样  版本对应

这里使用的和本机一样的sql安装包版本  

数据库路径：

`D:\BaiduNetdiskDownload\1____Javaweb黑马资料\day06-MySQL\day06-MySQL`



但是可视化工具由于无法联网所以只能选择hreisisql 需要适应



已经完成安装





后期导出访问量画图：

最简单的就是

##### 1，虚拟机命令行导出csv数据

##### 2，python本机画图



#### 4，前端环境安装✅

npm使用的是官网下载的 16.20.2

D:\softwareFile\nodsjs16202





nginx直接在黑马的文件夹下

D:\BaiduNetdiskDownload\1____Javaweb黑马资料\day03-Vue-Element\day03-Vue-Element\资料



这两个版本都是对的









#### 5，后端迁移运行✅

已经搞定了 



#### 6. delete和put不能用，需要修改所有接口✅

categorycontroller✅

admincontroller✅

userController✅

VideoController✅

VisitstatsController✅







用户端：

这两个只有get  无需修改

DisplayCategoryController✅

HoneController✅



登录端：

AdminLoginController✅

LoginController✅





确实是请求的问题   所有的delete和put已经修改为post

get请求暂时未修改



现在就是在本机测试，其他各个功能是否访问正常 尤其是同时上传的时候



#### 7，上线文案

各位同事，目前BIM视频网站已经部署至正式服务器，以下是相关信息：

**1. 网址：**
http://192.168.145.52:8090/login

在登录页面，分为普通用户和管理员账户。普通用户登录后可以观看视频，管理员则可通过点击“管理员登录”进入后台，进行视频上传和管理。

普通用户账户在首页注册，每位同事的初始管理员账户和上一次测试相同：

工号：工号

密码：1234

请大家登录后在管理页面修改自己的密码，确保账号安全。



**2. 用户功能：**
 普通用户登录后展示了我们内部上传的所有视频，以便提升其他员工对各类建模软件的学习效率。



**3. 管理员页面功能：**
 管理员页面是管理网站内容的地方，功能包括：

- 管理视频分类
- 上传、编辑、删除视频
- 编辑用户账户和管理员账户



**4. 视频上传：**
 请各位同事根据自己录制的软件，各自上传相关教学视频。

具体上传步骤如下：

1. 创建相应的视频分类。
2. 在视频管理页面上传视频，填写相关信息并选择对应的视频分类。

请各位同事在上传视频时，务必确认视频本身的格式和质量，确保视频能够正常播放：

1. **视频格式**：如 `.mov`（例如 MicroStation 的视频）无法正常播放，请尽量修改为常见且支持的格式，如 `.mp4` 等。
2. **视频质量**：请检查视频清晰度及声音效果，确保内容完整，可以拖拽且有声音。
3. **视频大小**：若视频较大，上传时可能需要稍作等待，上传过程中请耐心等候。





## 项目修改--登录改成域账户

李韬代码

```Java
package com.wisdri.epms.Util;

import com.wisdri.epms.Entity.Person;
import lombok.Data;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.PropertySource;
import org.springframework.stereotype.Service;

import javax.naming.AuthenticationException;
import javax.naming.Context;
import javax.naming.NamingEnumeration;
import javax.naming.directory.*;
import javax.naming.ldap.InitialLdapContext;
import javax.naming.ldap.LdapContext;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.Hashtable;
import java.util.List;

/**
 * @author 李韬 @date 2022/4/7 9:31
 * @description LDAP认证
 */
@Slf4j
@Service
@PropertySource(value = "classpath:application.properties", ignoreResourceNotFound = true)
public class LDAPUtil {

    /*
        使用范例：
        LDAPUtil util = new LDAPUtil();
		boolean flag = util.Authentication("14530", "Litaozuel123",
				"192.168.200.14", "@wisdri.com", "389");
		System.out.println(flag);
     */
    @Value("${LDAP.host}")
    public String host;
    @Value("${LDAP.domain}")
    public String domain;
    @Value("${LDAP.port}")
    public String port;
    @Value("${LDAP.enabled}")
    public String LDAPEnabled;


    /**
     * @param userName
     * @param password
     * @param host      IP
     * @param domain    @wisdri.com
     * @param port      默认389
     * @return
     */
    public LdapContext Authentication(String userName, String password, String host, String domain, String port){
        String url = new String("ldap://" + host + ":" + port);//固定写法
        String user = userName.indexOf(domain) > 0 ? userName : userName
                + domain;//网上有别的方法，但是在我这儿都不好使，建议这么使用
        Hashtable env = new Hashtable();//实例化一个Env
        LdapContext ctx = null;
        env.put(Context.SECURITY_AUTHENTICATION, "simple");//LDAP访问安全级别(none,simple,strong),一种模式，这么写就行
        env.put(Context.SECURITY_PRINCIPAL, user); //用户名
        env.put(Context.SECURITY_CREDENTIALS, password);//密码
        env.put(Context.INITIAL_CONTEXT_FACTORY,
                "com.sun.jndi.ldap.LdapCtxFactory");// LDAP工厂类
        env.put(Context.PROVIDER_URL, url);//Url
        //env.put(Context.REFERRAL, "follow");//这里有三种模式，默认是ignore
        try {
            ctx = new InitialLdapContext(env, null);
            //ctx1 = new InitialDirContext(env);// 初始化上下文
            log.info("初始化ctx");
            log.info(userName + " " + password + " " + "身份验证成功!");
            //readLdap(ctx, "dc=wisdri,dc=com", "cn=14530");
            return ctx;
        } catch (AuthenticationException e) {
            log.info(LocalDateTime.now() + " " + "身份验证失败!");
            e.printStackTrace();
            return null;
        } catch (javax.naming.CommunicationException e) {
            log.info(LocalDateTime.now() + " " + "AD域连接失败!");
            e.printStackTrace();
            return null;
        } catch (Exception e) {
            log.info(LocalDateTime.now() + " " + "未知的身份信息!");
            e.printStackTrace();
            return null;
        }
        //
        //一个非常狗血的问题：如果写了finally，try里面的内容一定会先走finally，再return
        //
//        finally{
//            if(null!=ctx){
//                try {
//                    ctx.close();
//                    ctx=null;
//                } catch (Exception e) {
//                    e.printStackTrace();
//                }
//            }
//        }
    }

    /**
     * 从ldap中读取用户基本信息
     * @param ctx
     * @param basedn
     * @param filter 过滤条件 格式：filter = "cn=14530";
     */
    public Person readLdap(LdapContext ctx, String basedn, String filter){
        Person person = new Person();
        try{
            if (ctx!=null){
                log.info("进入readLdap方法");
                //过滤条件
                //String filter = "displayName=李韬";//"(&(objectClass=*)(uid=*))"
                String[] attrPersonArray = { "uid", "userPassword", "displayName", "cn", "sn", "mail", "description",
                    "ou", "mobile", "objectClass"};

                //搜索控件
                SearchControls searchControls = new SearchControls();
                searchControls.setSearchScope(SearchControls.SUBTREE_SCOPE);//搜索范围
                searchControls.setReturningAttributes(attrPersonArray);
                System.out.println("filter="+filter + " baseDN=" + basedn);
                //可以查询所在组织
                NamingEnumeration<SearchResult> answer_test = ctx.search(basedn, filter.toString(), searchControls);
                //NamingEnumeration<SearchResult> answer_test = ctx.search("dc=wisdri,dc=com", "displayName=李韬", searchControls);
                while(answer_test!=null && answer_test.hasMoreElements()){
                    SearchResult sr = (SearchResult) answer_test.next();
                    log.info("getname=" + sr.getName());
                }

                //https://blog.csdn.net/belialxing/article/details/89157737
                //https://www.cnblogs.com/Nadim/p/4681003.html
                //https://blog.csdn.net/qq_34605063/article/details/108303657
                // 1.要搜索的上下文或对象的名称；
                // 2.过滤条件，可为null，默认搜索所有信息；
                // 3.搜索控件，可为null，使用默认的搜索控件
                NamingEnumeration<SearchResult> answer = ctx.search(basedn, filter.toString(),searchControls);
                log.info("搜索结果是否为空？" + (answer == null) + " hasMore?" + (answer.hasMore()) + "  " + answer.toString());
                while (answer.hasMore()) {
                    log.info("执行搜索ing...");
                    SearchResult result = (SearchResult) answer.next();
                    NamingEnumeration<? extends Attribute> attrs = result.getAttributes().getAll();

                    while (attrs.hasMore()) {
                        Attribute attr = (Attribute) attrs.next();
                        log.info(attr.getID() + " " + attr.get().toString());
                        if("cn".equals(attr.getID())){
                            System.out.println(attr.get().toString());
                            person.setId(attr.get().toString());
                        }else if("displayName".equals(attr.getID())){
                            person.setName(attr.get().toString());
                        }else if("mail".equals(attr.getID())){
                            person.setMail(attr.get().toString());
                        }
                    }
                }
            }
        }catch (Exception e) {
            System.out.println("获取用户信息异常:");
            e.printStackTrace();
        }

        return person;
    }
}
```

### 1，修改梳理

#### （1）后端--pom文件

添加需要的依赖

```xml
   <dependency>
            <groupId>org.springframework.ldap</groupId>
            <artifactId>spring-ldap-core</artifactId>
            <version>2.3.4.RELEASE</version>
        </dependency>
```



#### （2）后端--LDAPUtils

```java 
package com.example.wisdrivideo.Utils;

import lombok.Data;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.PropertySource;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;

import javax.naming.AuthenticationException;
import javax.naming.Context;
import javax.naming.NamingEnumeration;
import javax.naming.directory.*;
import javax.naming.ldap.InitialLdapContext;
import javax.naming.ldap.LdapContext;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.Hashtable;
import java.util.List;

/**
 * @author 李韬
 * @date 2022/4/7 9:31
 * @description LDAP认证工具类（已精简，仅用于身份验证）
 */
@Slf4j
@Component
@PropertySource(value = "classpath:application.properties", ignoreResourceNotFound = true)
public class LDAPUtil {

    @Value("${LDAP.host}")
    private String host;

    @Value("${LDAP.domain}")
    private String domain;

    @Value("${LDAP.port}")
    private String port;

    /**
     * 使用配置文件中的信息验证用户身份
     *
     * @param userName LDAP 用户名（工号）
     * @param password LDAP 密码
     * @return 是否认证成功
     */
    public boolean authenticate(String userName, String password) {
        String url = "ldap://" + host + ":" + port;
        String user = userName.contains(domain) ? userName : userName + domain;

        Hashtable<String, String> env = new Hashtable<>();
        env.put(Context.SECURITY_AUTHENTICATION, "simple");
        env.put(Context.SECURITY_PRINCIPAL, user);
        env.put(Context.SECURITY_CREDENTIALS, password);
        env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
        env.put(Context.PROVIDER_URL, url);

        try {
            LdapContext ctx = new InitialLdapContext(env, null);
            if (ctx != null) {
                log.info("LDAP身份验证成功: {}", userName);
                ctx.close(); // 释放资源
                return true;
            }
        } catch (AuthenticationException e) {
            log.warn("LDAP身份验证失败: {}，原因: 身份认证失败", userName);
        } catch (javax.naming.CommunicationException e) {
            log.error("LDAP连接失败: {}", url);
        } catch (Exception e) {
            log.error("LDAP身份验证发生异常: {}", e.getMessage());
        }
        return false;
    }
}

```

#### （3）后端-- Controller和service

mapper这里不需要修改 因为不走数据库了

Controller：

```

```

service：将login直接重写

```Java
@Override
    public User login(User user) {
        String username = user.getUsername();
        String password = user.getPassword();

        // 使用 LDAP 验证身份
        boolean isAuthenticated = ldapUtil.authenticate(username, password);

        if (!isAuthenticated) {
            log.warn("LDAP 登录失败：{}", username);
            throw new IllegalArgumentException("用户名或密码错误");
        }

        // 构造一个用于后续 token 返回的 User 对象（不查数据库）
        User loginUser = new User();
        loginUser.setUsername(username);
        loginUser.setUserId(1L); // 你之前是用 userId 做唯一标识，这里先用用户名充当
        loginUser.setIsAdmin(false); // 默认设为普通用户，必要时你可以扩展从LDAP取角色

        log.info("LDAP 登录成功：{}", username);
        return loginUser;
    }
```

#### （4）前端--删除用户注册接口

没啥说的  

#### （5）登录页面--字体微调 背景图修改

这里还是比较繁琐的 需要先在本机代码上修改

然后挨个复制到Linux的文件中



#### （6）用户管理接口隐藏

没啥说的 直接隐藏就完事了





#### （7）上线文案

各位同事，目前 **BIM视频网站网址：** http://192.168.145.52:8090/login

系统已调整为**使用公司域账户登录**，普通用户无需注册，直接输入工号与域密码即可登录观看视频。

**管理员登录方式保持不变**，初始账户信息如下：

> 工号：工号
>  密码：1234

请各位负责录制视频的同事使用管理员账户登录后台，根据录制内容创建视频分类，并上传相关教学视频，便于后续内容展示与BIM培训使用，谢谢。











# 25.8.28Java网站修改

#### 0，尝试跑通

已经将项目捡起

直接在wisdrivideoLinux里面跑即可



后端的静态资源映射需要调整



前端暂无



几个post不对应的已经搞定了



这里有两个前端 后端只有一个Linux里的那个

前端wisdrivue是本机

wisdeiLinuxvue是修改之后放到服务器上的



#### 1，大视频无法上传问题

检查了一天  双重问题 

本机设置+浏览器内核限制  无法通过简单的设置解决

如果需要解决，需要重新开发分块上传的功能 前后端一起  但是这边不给时间开发 放弃  

通过上传者将超过1.5G的视频切片解决



#### 2，标签页不对齐问题

```css
video-item {
  margin: 10px 0;
  display: block; /* 原先是 flex；改成 block，避免外层再做一次左右分布 */
}

/* 2) 关键：内部左右两列（标题、时长）的容器 */
.video-details {
  display: grid;
  grid-template-columns: 1fr auto; /* 左边自适应，右边内容宽度自适应 */
  column-gap: 8px;                /* 左右留点间距 */
  align-items: start;              /* 顶对齐 */
  width: 100%;
}

/* 3) 左侧标题：占满剩余空间，允许换行 */
.video-link {
  word-break: break-word;   /* 避免太长不换行 */
  text-align: left;         /* 强制左对齐 */
  display: block;           /* 允许多行 */
  min-width: 0;               /* 防止过长内容把右侧挤出 */
  word-break: break-word;     /* 或者 overflow-wrap: anywhere; 都行 */
           /* 保证可多行换行 */
  font-size: 16px;
  color: #333;
  text-decoration: none;
  padding: 5px 0;
  transition: color 0.3s, transform 0.3s;
}


/* 4) 右侧时长：固定宽度、右对齐、不换行、等宽数字对齐更稳 */
.video-duration {
  flex: 0 0 5ch;              /* 固定列宽，5 个字符的宽度，够放 0:52 / 12:30 等 */
  text-align: right;
  white-space: nowrap;        /* 禁止换行 */
  font-variant-numeric: tabular-nums; /* 数字等宽，更整齐（浏览器支持则生效） */
  color: #888;
}
```

改成了这样 应该是对齐了



不对 这样还有进度时间不对应的问题

在displaycategoryView里面  399行开始  修改下面这四个样式

```css
/* 视频项样式 */
.video-item {
  margin: 10px 0;
  display: flex;
  justify-content: space-between; /* 左右两部分对齐 */
  align-items: center;
  padding: 5px 0;      /* ← 新增：把上下留白放到条目容器 */
}

/* 2) 关键容器：两列网格——左列自适应，右列固定宽度 */
.video-details {
  display: grid !important;
  grid-template-columns: 1fr 64px; /* 右列固定 64px，更稳；两处数值要一致 */
  column-gap: 12px;
  align-items: start;   /* 顶对齐 */
  width: 100%;
  box-sizing: border-box;
}

/* 3) 左侧标题：多行、左对齐，占满剩余 */
.video-link {
  display: block;
  min-width: 0;
  text-align: left;
  overflow-wrap: anywhere;   /* 或 word-break: break-word */
  margin: 0;
  line-height: 1.4;
  font-size: 16px;
  color: #333;
  text-decoration: none;
  transition: color 0.3s, transform 0.3s;
}

/* 4) 右侧时长：固定列、靠右、不换行、数字等宽 */
.video-duration {
  justify-self: end;
  width: 64px;                
  text-align: right;
  white-space: nowrap;

  /* 用和正文接近的字体，而不是纯 monospace */
  font-family: 'Microsoft YaHei', 'PingFang SC', 'Helvetica Neue', Arial, sans-serif;

  /* 关键：启用字体的等宽数字特性 */
  font-variant-numeric: tabular-nums;
  font-feature-settings: "tnum" 1;

  color: #888;
  line-height: 1.4;
}

```



#### 3，前端字体 颜色 背景图替换等展示修改问题

杜工给出了新的图片 按照修改

底图背景#FFFFFF，标题栏#F7F7F7，字体就是默认的

右上角的图标不要颜色，字体改为#12B56A



保险起见 这是之前的样式  留档

```css
<style scoped>
/* Reset default margin and padding */
html, body {
    margin: 0;
    padding: 0;
    height: 100%;
    box-sizing: border-box;
}

/* Ensure the body takes up at least the full viewport height */
body {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}

/* 头部样式 */
.user-header {
  display: flex;
  align-items: center;
  padding: 2vh 2vw;
  background-color: #CBB594;
  color: #000000;
  box-shadow: 0 2px 5px rgba(143, 137, 137, 0.1);
}

.header-content {
  display: flex;
  width: 100%;
  align-items: center;
}

/* 左侧区域 */
.left-section {
  display: flex;
  align-items: center;
  justify-content: flex-start; /* 保证 logo 从左侧开始 */
  flex: 0 0 20%; /* 确保它的宽度不自动拉伸 */
  padding: 0; /* 去除内边距 */
  margin: 0; /* 去除外边距 */
}

.logo-img {
  width: 13vw;  /* 使用视口宽度来设置宽度，10% 的屏幕宽度 */
  height: auto;  /* 高度自适应 */
  object-fit: contain;  /* 保证图片按比例缩放 */
}

.middle-section {
  display: flex;
  flex-direction: column; /* 使得标题和英文翻译垂直排列 */
  align-items: center;
  justify-content: center; /* 保证标题居中 */
  flex: 1 1 40%; /* 让它占据剩余空间 */
  padding-left: 9vw;  /* 根据需要调整左侧空隙 */
  padding-right: 1vw; /* 根据需要调整右侧空隙 */
}

.middle-section h1 {
  font-size: 1.8vw; 
  font-weight: bold; 
  color: #000307; 
  margin: 0; 
  text-align: center; 
  font-family: 'KaiTi', 'STSong', 'STKaiti', serif; /* 字体堆栈 */ 
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2); /* 增加字体阴影，增强加粗效果 */
}

.subtitle {
  font-size: 0.8rem; /* 英文翻译字体大小 */
  font-weight: bold; 
  color: #000307; /* 英文翻译颜色 */
  margin-top: 0.5rem; /* 增加一些间距 */
  text-align: center; /* 英文翻译居中 */
  position: absolute; /* 绝对定位，确保英文位置不影响中文 */
  top: 11%; /* 英文翻译位于中文标题的下方，这个值根据需要调整 */
  transform: translateX(-10%); /* 水平居中 */
  transform: scaleX(1.1); /* 拉伸英文文字 */
  white-space: nowrap; /* 防止英文换行 */
  transition: all 0.3s ease; /* 加入过渡效果 */
  letter-spacing: 0.18em; /* 通过字母间距增加空白 */
  font-family: 'Microsoft YaHei', 'FangSong', 'Songti SC', 'KaiTi', 'SimSun', serif; /* 英文部分也使用类似的字体 */
}



.right-section {
  display: flex;
  flex: 1 1 30%;
  align-items: center;
  
}

.right-section {
  justify-content: flex-end; 
}

.user-info {
  display: flex;
  align-items: center;
  margin-right: 0.6vw;
}

.welcome-text {
  font-size: 1.2rem;
  margin-right: 0.6vw;
  font-weight: bold; 
  font-family: 'KaiTi', 'STSong', 'STKaiti', serif; /* 字体堆栈 */ 
  text-shadow: 1px 1px 1px rgba(0, 0, 0, 0.1); /* 增加字体阴影，增强加粗效果 */
}


.user-avatar {
  width: 2.5rem;
  height: 2.5rem;
  border-radius: 50%;
}

/* 通用按钮样式 */
.button {
  padding: 0.5rem 1.2rem; /* 按钮的内边距 */
  font-size: 1rem; /* 按钮文字的大小 */
  font-weight: bold; /* 字体加粗 */
  border-radius: 10px; /* 圆角效果 */
  margin-left: 2px; /* 保证按钮之间有间距 */
  background-color: #B35A5A; /* 设置按钮的背景色 */
  color: #020202; /* 字体颜色 */
  border: none; /* 去掉按钮的默认边框 */
  transition: all 0.3s ease; /* 按钮交互效果 */
  font-family: 'Microsoft YaHei', 'FangSong', 'Songti SC', 'KaiTi', 'SimSun', serif; /* 英文部分也使用类似的字体 */
}

.button:hover {
  background-color: #9e4a4a; /* 悬停时背景色稍微变深 */
  
}

.button:active {
  transform: translateY(1px); /* 点击时按钮下沉 */
  background-color: #8a3e3e; /* 点击时背景色进一步加深 */
}


.category-container {
  display: flex;
  flex-wrap: wrap;          /* 支持换行 */
  justify-content: flex-start;  /* 每行的卡片从左对齐 */
  padding: 0 15vw; /* 左右留出 10% 的空白区域 */
  gap: 6px;  /* 控制卡片之间的间距 */
  margin-top: 0px; /* 给上面加一点间距 */
  background-color: #EBE1CD;
}

.page-content{
  background-color: #EBE1CD;
  min-height: 100vh; /* 确保至少填满整个视口 */
  display: flex;
  flex-direction: column;
  justify-content: flex-start; /* 让内容从顶部开始 */
}




/* 分割线样式 */
.divider {
  border-top: 2px solid #665e5e; /* 设置横线，颜色为浅灰色 */
  margin: 2vh 0; /* 上下间距 */
}


/* 播放窗口容器，设置最大宽度为屏幕的50% */
.main-content {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  justify-content: flex-start;
  align-items: flex-start;
  gap: 20px; /* 给两个区域之间加间隔 */
  align-items: flex-start; /* 让视频列表和播放器顶部对齐 */
  width: 100%;
}

.video-player-container {
  width: 60%; /* 播放区域占据大部分空间 */
  padding-right: 1%;
  margin-left: 10%; /* 增加左边的间隔，拉视频窗口向右 */
  
}


.video-player-container h2 {
  font-size: 1.8vw; 
  font-weight: bold; 
  margin-top: 4vh;  /* 增大顶部间距 */
  margin-bottom: 2vh;  /* 增大底部间距 */
  color: #000307; 
  
  text-align: left; 
  font-family: 'KaiTi', 'STSong', 'STKaiti', serif; /* 字体堆栈 */ 
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3); /* 增加字体阴影，增强加粗效果 */
}

.video-list-container {
  width: 20%; /* 视频列表占据较小空间 */
  padding-left: 2%;
  padding-right: 2%;
  margin-top: 10vh;
  height: 60vh; /* 设置视频列表的最大高度 */
  background-color: #fff; /* 白色背景 */
  border-radius: 10px; /* 圆角边框 */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1); /* 添加阴影 */
  overflow-y: auto; /* 当内容溢出时显示滚动条 */
  align-items: flex-start; /* 确保视频列表容器顶部对齐 */
}

.video-title {
  font-size: 1.6vw; 
  font-weight: bold; 
  margin-top: 4vh;  /* 增大顶部间距 */
  margin-bottom: 2vh;  /* 增大底部间距 */
  color: #000307; 
  text-align: left; 
  font-family: 'KaiTi', 'STSong', 'STKaiti', serif; /* 字体堆栈 */ 
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3); /* 增加字体阴影，增强加粗效果 */
}

.video-list {
  list-style-type: none;
  padding-left: 0;
}

/* 视频项样式 */
.video-item {
  margin: 10px 0;
  display: flex;
  justify-content: space-between; /* 左右两部分对齐 */
  align-items: center;
  padding: 5px 0;      /* ← 新增：把上下留白放到条目容器 */
}

/* 2) 关键容器：两列网格——左列自适应，右列固定宽度 */
.video-details {
  display: grid !important;
  grid-template-columns: 1fr 64px; /* 右列固定 64px，更稳；两处数值要一致 */
  column-gap: 12px;
  align-items: start;   /* 顶对齐 */
  width: 100%;
  box-sizing: border-box;
}

/* 3) 左侧标题：多行、左对齐，占满剩余 */
.video-link {
  display: block;
  min-width: 0;
  text-align: left;
  overflow-wrap: anywhere;   /* 或 word-break: break-word */
  margin: 0;
  line-height: 1.4;
  font-size: 16px;
  color: #333;
  text-decoration: none;
  transition: color 0.3s, transform 0.3s;
}

/* 4) 右侧时长：固定列、靠右、不换行、数字等宽 */
.video-duration {
  justify-self: end;
  width: 64px;                
  text-align: right;
  white-space: nowrap;

  /* 用和正文接近的字体，而不是纯 monospace */
  font-family: 'Microsoft YaHei', 'PingFang SC', 'Helvetica Neue', Arial, sans-serif;

  /* 关键：启用字体的等宽数字特性 */
  font-variant-numeric: tabular-nums;
  font-feature-settings: "tnum" 1;

  color: #888;
  line-height: 1.4;
}



.video-link:hover {
  color: #21A675;  /* 鼠标悬停时字体变为绿色 */
  transform: translateX(5px);
}

/* 当前播放的视频标签，持续变绿 */
.video-link.playing {
  color: #21A675;  /* 当前播放的视频字体始终为绿色 */
}

.video-player-container video {
  width: 100%; /* 播放器宽度自适应 */
  height: auto;
}



.home-button,
.logout-button {
  margin-left: 10px;
}

@media screen and (max-width: 768px) {
  .video-list {
    grid-template-columns: repeat(2, 1fr);
    /* 小屏幕一行展示2个 */
  }
}

@media screen and (max-width: 480px) {
  .video-list {
    grid-template-columns: repeat(1, 1fr);
    /* 手机屏幕一行展示1个 */
  }
}
</style>
```

![image-20250829144353101](C:/Users/14088/AppData/Roaming/Typora/typora-user-images/image-20250829144353101.png)

![image-20250829144358032](C:/Users/14088/AppData/Roaming/Typora/typora-user-images/image-20250829144358032.png)



![image-20250902161952850](C:/Users/14088/AppData/Roaming/Typora/typora-user-images/image-20250902161952850.png)





这里的样式直接将本机的运行代码复制到服务器版本

前端的代码直接复制到服务器吧  修改的小地方太多了不如直接复制

三个部分：

登录页面，视频首页，视频播放页







现在的播放页版本

```vue
<template>
  <div>
    <!-- 用户头部 -->
    <!-- 用户头部 -->
    <header class="user-header">
      <div class="header-content">
        <!-- 左侧：logo -->
        <div class="left-section">
          <div class="logo">
            <img :src="require('@/assets/logoword.png')" alt="logo" class="logo-img" />
          </div>
        </div>

        <!-- 中间：页面标题（已整体注释掉，不显示） -->
        <!-- 
        <div class="middle-section">
          <h1>中冶南方BIM讲堂</h1>
          <p class="subtitle">WISDRI&nbsp;&nbsp;BIM&nbsp;&nbsp;ACADEMY</p>
        </div>
        -->

        <!-- 右侧：用户信息和退出登录按钮 -->
        <div class="right-section">
          <div class="user-info">
            <span class="welcome-text">欢迎您，{{ username }}</span>
          </div>
          <el-button class="home-button button" @click="goHome">首页</el-button>
          <el-button class="logout-button button" @click="handleLogout">退出登录</el-button>
        </div>
      </div>
    </header>

   <div class="page-content">
    <!-- 播放窗口 -->
   <div class="main-content">
      <!-- 播放窗口 -->
      <div class="video-player-container" v-if="selectedVideo">
        <div class="title-row">
          <h1 class="title-main">课程详情</h1>
          <h2 class="title-sub">正在播放：{{ selectedVideo.title }}</h2>
        </div>

        <!-- 绑定 videoUrl -->
        <video ref="videoPlayer" controls :src="videoUrl" width="100%" height="auto" controlsList="nodownload" />
      </div>

      <!-- 视频列表 -->
      <div class="video-list-container">
        <h1 class="video-title">视频列表</h1>

        <!-- 添加分割线 -->
        <div class="divider"></div>

        <ul class="video-list">
          <li v-for="video in videos" :key="video.id" class="video-item">
            <div class="video-details">
              <a 
                href="javascript:void(0);" 
                @click="selectVideo(video)" 
                class="video-link" 
                :class="{'playing': selectedVideo && selectedVideo.id === video.id}">
                {{ video.title }}
              </a>
              <span class="video-duration">{{ video.durationFormatted }}</span>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>

    <!-- 页面底部 -->
    <footer class="page-footer">
        <img :src="require('@/assets/wisdrilogo.png')" alt="logo" class="footer-logo" />
        <span class="footer-text">中冶南方钢铁公司工程数字化(BIM)中心</span>
    </footer>

  </div>
</template>

<script setup>
import { useRouter, useRoute } from 'vue-router';
import { ref, onMounted } from 'vue';
import Request from '@/utils/request';

const router = useRouter();
const route = useRoute();
const username = ref('');
const videos = ref([]);

// 当前选中的视频
const selectedVideo = ref(null);
// 播放器视频地址
const videoUrl = ref('');
const baseUrl = process.env.VUE_APP_API_URL;

/**
 * 统一生成播放 URL 的函数
 * 👉 现在：保持直链
 * 👉 以后：只改这里，比如去请求签名接口，返回假URL
 */
const getPlayUrl = (filePath) => {
  const norm = String(filePath || '')
    .replace(/\\/g, '/')   // 反斜杠 -> 正斜杠
    .replace(/^\.\/?/, ''); // 去掉开头 ./ 或 .\
  return `${baseUrl}/${norm}`;
};

onMounted(async () => {
  const storedUsername = sessionStorage.getItem('username');
  const token = sessionStorage.getItem("token");
  if (storedUsername && token) {
    username.value = storedUsername;
  } else {
    username.value = storedUsername || "未登录";
    return router.push("/login");
  }

  const categoryId = route.params.categoryId;
  if (!categoryId) {
    console.error('categoryId 不存在');
    return;
  }

  try {
    const response = await Request.get(`/user/Home/${categoryId}/videos`);

    // 初始化视频列表
    videos.value = response.data.map((video) => ({
      ...video,
      durationFormatted: '加载中...'
    }));

    console.log(videos.value);

    // 获取视频时长（用 getPlayUrl）
    videos.value.forEach(video => {
      const videoElement = document.createElement('video');
      videoElement.preload = 'metadata';
      videoElement.src = getPlayUrl(video.filePath);

      videoElement.onloadedmetadata = () => {
        video.durationFormatted = formatDuration(videoElement.duration);
        videoElement.remove(); // 释放临时 video
      };
      videoElement.onerror = () => {
        video.durationFormatted = '未知';
        videoElement.remove();
      };
    });

    // 默认播放第一个视频
    if (videos.value.length > 0) {
      selectVideo(videos.value[0]);
    }
  } catch (error) {
    console.error('加载视频数据失败:', error);
  }
});

// 选择视频并显示播放器（用 getPlayUrl）
const selectVideo = (video) => {
  selectedVideo.value = video;
  videoUrl.value = getPlayUrl(video.filePath);
};

// 格式化视频时长
const formatDuration = (duration) => {
  if (typeof duration !== 'number' || isNaN(duration) || duration <= 0) {
    return '未知';
  }
  const minutes = Math.floor(duration / 60);
  const seconds = Math.floor(duration % 60);
  return `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
};

const handleLogout = () => {
  sessionStorage.removeItem('username');
  localStorage.removeItem('token');
  router.push('/login');
};

const goHome = () => {
  router.push('/');
};
</script>
```



#### 现在前端获取视频的方式

1. ##### 先拿 “视频列表”（主动问后端要信息）

页面刚加载时，你会主动调用一个接口，问后端 “这个分类下有哪些视频？”



- 代码里的关键：`await Request.get(`/user/Home/${categoryId}/videos`)`
- 你问后端要的不是视频本身，而是 “视频的基本信息”（比如视频 ID、标题、真实存储路径 `filePath`）
- 举个例子：后端会返回类似 `[{id:123, title:"第一课", filePath:"./videos/lesson1.mp4"}, ...]` 的数据

2. ##### 选视频时 “拼地址”（把真实路径变成可访问的 URL）

当你点击某个视频（或页面自动选第一个），会执行 `selectVideo` 函数：



- 这个函数会把后端给的 `filePath`（比如 `./videos/lesson1.mp4`），加工成浏览器能访问的地址
- 加工逻辑：后端基础地址（比如 `http://你的后端IP:8080`） + 处理后的 `filePath`，拼成最终地址（比如 `http://你的后端IP:8080/videos/lesson1.mp4`）
- 这个最终地址会存到 `videoUrl` 变量里

3. ##### 浏览器 “自动请求”（`video` 标签加载地址）

你在模板里写了 `<video :src="videoUrl">`，意思是 “视频标签的播放地址，用 `videoUrl` 这个变量的值”



- 当 `videoUrl` 有值（比如上面拼好的 `http://.../lesson1.mp4`），浏览器会 **自动去请求这个地址**，把视频文件拉过来播放
- 你没写额外的 “发送请求” 代码，是浏览器看到 `video` 标签有 `src` 后，自己帮你发的请求



### swagger链接

```
http://192.168.205.234:8080/doc.html#/home
```



### 修改视频分类名问题修改：

#### 1，通过将视频文件夹目录改成id 解决修改视频分类名之后，视频路径不对应的问题

已经解决  加了一堆接口

对于已经部署的文件夹解决：

##### 0，手动修改服务器的文件夹 将文件夹名称改成视频分类的id

这个自己去看数据库改吧

通过swagger里面的：

##### 1，视频相关接口：

按分类id批量把file_path改。。。  将视频的filepath修改正确

自己看接口的名字来判断即可

##### 2，视频分类相关接口：

（1）预览--查看需要修改的  是的有这个接口

（2）批量修复 将所有视频路径改回id格式



#### 2，解决后端现有视频路径错误的问题

应该已经通过接口的方式解决了，现在只需要手动修改现有的文件夹名称 其他的都可以通过数据库解决！

具体的自己去服务器探究吧 忘了！







1，出差及相关准备与设备调试：完成行程安排和前期准备，开展数据测量，学习设备使用方法，并对现场出现的 Bug 进行调试（4 天）。

 2，视频网站视频流处理：完成FFmpeg工具安装，学习基本使用方法，并编写部分后端接口进行对接（1 天）。

















1，视频分类/上传路径不一致 Bug 修复：已统一为按分类 ID 存储，并同步调整前后端接口（1.5 天）。
2，视频链接隐藏问题解决：
方案一：将真实 URL 映射为假链接并由后端解析，结果仍可直接下载（1 天）；
方案二：改用 Blob 链接 + 限时 key，虽实现了 blob+206 链接，但在有效期内仍能下载（2天）；
方案三：调研尝试用第三方工具 ffmpeg 切片方式解决。



直接上方案三的笔记：







# 视频切片防下载｜最小实现方案（可直接交付）

## 目标

- 前端不再使用 `blob:` 或 `.mp4` 直链；统一改为 **HLS**（`.m3u8 + .ts`）。
- F12 网络面板只出现清单与切片，**没有整段 MP4** 链接。
- **兼容历史视频**：数据库中已有的 `source_path`（MP4 路径）不变；切片按需生成并长期复用。
- 代码量最小；每一步可在 **Swagger** 上验证。

## 存储与目录

- 源视频（保持不动）：仍按现状，例如 `/data/raw/<分类>/<文件>.mp4`（DB已有 `source_path`）。
- 切片目录（新增，只放 HLS）：`/data/hls/cat_<categoryId>/vid_<videoId>/index.m3u8 + seg-00000.ts...`
- 每个视频一个独立目录；后续可对 `/data/hls` 做 LRU 清理（只删切片，不删源）。

## 数据库最小改造（仅新增）

- `hls_ready` tinyint (0/1)：是否已切片。
- `hls_dir` varchar：相对路径，如 `cat_12/vid_ab12cd/`（相对 `/data/hls`）。

> 已有的 `source_path`、`category_id` 等字段保持不变。

## 后端接口（Spring Boot）

1. `POST /api/hls/prepare`
   - 入参：`videoId`（或主键）。
   - 行为：查询 DB → 若 `/data/hls/cat_<cid>/vid_<vid>/index.m3u8` 已存在，直接返回；否则调用 **ffmpeg** 生成切片（先 `-c copy`，失败再转码），完成后返回。
   - 返回：`{status:"ready", m3u8:"/hls/cat_<cid>/vid_<vid>/index.m3u8"}` 或 `{"status":"preparing"}`（可选轮询）。
   - **Swagger 验证**：填 `videoId` 调用，拿到 `m3u8` 即通过。
2. `GET /hls/**`（起步版为**静态映射只读**到 `/data/hls`，便于先跑通）
   - **Swagger/浏览器验证**：访问返回的 `index.m3u8` 能看到文本清单。

> 说明：为降低复杂度，起步阶段**不做会话签名/分段鉴权**；若后续需要再加，不影响现有结构。

## Nginx（两条必要规则）

```
# 禁止整段视频直链
location ~* \.(mp4|mkv|mov|avi)$ { deny all; }

# 仅放行 HLS
location /hls/ {
  proxy_pass http://127.0.0.1:8080/hls/;
  proxy_read_timeout 300s;
}
```

**验收**：直接访问任意历史 `.mp4` 应返回 403/404。

## 前端改动（极简）

- 移除 `fetch→blob→URL.createObjectURL`。
- 播放流程：进入播放页 → 调 `POST /api/hls/prepare` → 取 `m3u8` →
  - Chrome/Edge/Firefox：本地 `hls.js` 加载 `m3u8`；
  - Safari/iOS：原生 `<video src="...m3u8">`。
     **验收**：F12 只见 `.m3u8` 与多条 `.ts` 请求，无 `.mp4`、无 `blob:`。

## 可选项：前端可见水印（简单开关）

- 在视频容器上叠加半透明文本（用户名@时间），缓慢漂移（CSS 动画/定时）。
   **验收**：观看时肉眼可见；截图/屏录也带水印。

## 兼容性与并发

- **完全兼容**现有数据库与已上传视频；源文件不动。
- 支持“热门预切 + 冷门懒切”；多人观看**共用同一套切片**，不重复生成。
- 如担心并发触发重复切片，可在后端加“视频级互斥锁”（后续再加不影响本方案）。

## 一次性验收顺序（Swagger/浏览器）

1. `POST /api/hls/prepare`（传一个已有 `source_path` 的 `videoId`）→ 返回 `status=ready` 与 `m3u8`。
2. 浏览器访问 `m3u8` → 能看到清单文本。
3. 前端用 `m3u8` 播放 → F12 仅 `.m3u8` + `.ts`。
4. 访问历史 `.mp4` 直链 → 403/404。
5. （可选）打开水印 → 页面出现漂移文字水印。

------

把这段文字发给我（或未来的我），我就能直接基于它输出：

- `POST /api/hls/prepare` 的后端最小代码 + ffmpeg 调用；
- `/hls/**` 静态映射配置；
- Nginx 片段；
- 前端 `hls.js` 接入与简易水印样式。

#### 1，ffmpeg

已经在官网下载

安装和配置连接如下：

[FFmpeg 超级详细安装与配置教程（Windows 系统）_windows安装ffmpeg-CSDN博客](https://blog.csdn.net/Natsuago/article/details/143231558)

本地已经测试完成

#### 2,代码部分



##### 1，数据库修改

```sql
# 1. 打开CMD，连接MySQL
mysql -h localhost -u root -p

# 2. 输入密码后，选择数据库
SHOW DATABASES;
USE mydatabase;

# 3. 查看所有表

SHOW TABLES;

# 4. 查看表结构
DESCRIBE users;

# 5. 查询数据
SELECT * FROM users;


# 9. 退出
EXIT;
```



先备份数据库：！！！

```sql
CREATE TABLE videos_backup SELECT * FROM videos;
```



```sql
/* ========= HLS 切片改造 · 一次性脚本（含事务与回滚） ========= */
DELIMITER $$
 
DROP PROCEDURE IF EXISTS migrate_videos_hls $$
CREATE PROCEDURE migrate_videos_hls()
BEGIN
  /* 任何 SQL 异常都会回滚并抛出 */
  DECLARE EXIT HANDLER FOR SQLEXCEPTION
  BEGIN
    ROLLBACK;
    RESIGNAL;
  END;

  START TRANSACTION;

  /* ---- 1) 按需新增字段（存在就跳过） ---- */
  IF NOT EXISTS (
    SELECT 1 FROM INFORMATION_SCHEMA.COLUMNS
     WHERE TABLE_SCHEMA = DATABASE() AND TABLE_NAME='videos' AND COLUMN_NAME='video_uid'
  ) THEN
    ALTER TABLE `videos`
      ADD COLUMN `video_uid` CHAR(32) NULL COMMENT '视频短ID(32位hex, 目录命名用)' AFTER `id`;
  END IF;

  IF NOT EXISTS (
    SELECT 1 FROM INFORMATION_SCHEMA.COLUMNS
     WHERE TABLE_SCHEMA = DATABASE() AND TABLE_NAME='videos' AND COLUMN_NAME='hls_ready'
  ) THEN
    ALTER TABLE `videos`
      ADD COLUMN `hls_ready` TINYINT(1) NOT NULL DEFAULT 0 COMMENT '是否已切片(0=否,1=是)' AFTER `file_name`;
  END IF;

  IF NOT EXISTS (
    SELECT 1 FROM INFORMATION_SCHEMA.COLUMNS
     WHERE TABLE_SCHEMA = DATABASE() AND TABLE_NAME='videos' AND COLUMN_NAME='hls_dir'
  ) THEN
    ALTER TABLE `videos`
      ADD COLUMN `hls_dir` VARCHAR(255) NULL COMMENT 'HLS相对目录: cat_<cid>/vid_<uid8>/' AFTER `hls_ready`;
  END IF;

  /* 可选：便于前端展示/统计；不影响最小方案 */
  IF NOT EXISTS (
    SELECT 1 FROM INFORMATION_SCHEMA.COLUMNS
     WHERE TABLE_SCHEMA = DATABASE() AND TABLE_NAME='videos' AND COLUMN_NAME='duration_seconds'
  ) THEN
    ALTER TABLE `videos`
      ADD COLUMN `duration_seconds` INT NULL COMMENT '时长(秒)' AFTER `hls_dir`;
  END IF;

  IF NOT EXISTS (
    SELECT 1 FROM INFORMATION_SCHEMA.COLUMNS
     WHERE TABLE_SCHEMA = DATABASE() AND TABLE_NAME='videos' AND COLUMN_NAME='width'
  ) THEN
    ALTER TABLE `videos`
      ADD COLUMN `width` INT NULL COMMENT '宽(px)' AFTER `duration_seconds`;
  END IF;

  IF NOT EXISTS (
    SELECT 1 FROM INFORMATION_SCHEMA.COLUMNS
     WHERE TABLE_SCHEMA = DATABASE() AND TABLE_NAME='videos' AND COLUMN_NAME='height'
  ) THEN
    ALTER TABLE `videos`
      ADD COLUMN `height` INT NULL COMMENT '高(px)' AFTER `width`;
  END IF;

  /* ---- 2) 回填历史数据 ---- */
  /* video_uid：为空就填一个去横线的 UUID */
  UPDATE `videos`
     SET `video_uid` = REPLACE(UUID(), '-', '')
   WHERE `video_uid` IS NULL OR `video_uid` = '';

  /* hls_dir：为空就按规则生成  cat_<category_id>/vid_<video_uid前8位>/ */
  UPDATE `videos`
     SET `hls_dir` = CONCAT('cat_', COALESCE(`category_id`, 0), '/vid_', LOWER(LEFT(`video_uid`, 8)), '/')
   WHERE (`hls_dir` IS NULL OR `hls_dir` = '')
     AND `video_uid` IS NOT NULL AND `video_uid` <> '';

  /* ---- 3) 索引（不存在就创建） ---- */
  IF NOT EXISTS (
    SELECT 1 FROM INFORMATION_SCHEMA.STATISTICS
     WHERE TABLE_SCHEMA = DATABASE() AND TABLE_NAME='videos' AND INDEX_NAME='uk_video_uid'
  ) THEN
    ALTER TABLE `videos` ADD UNIQUE KEY `uk_video_uid` (`video_uid`);
  END IF;

  IF NOT EXISTS (
    SELECT 1 FROM INFORMATION_SCHEMA.STATISTICS
     WHERE TABLE_SCHEMA = DATABASE() AND TABLE_NAME='videos' AND INDEX_NAME='idx_category_id'
  ) THEN
    ALTER TABLE `videos` ADD INDEX `idx_category_id` (`category_id`);
  END IF;

  IF NOT EXISTS (
    SELECT 1 FROM INFORMATION_SCHEMA.STATISTICS
     WHERE TABLE_SCHEMA = DATABASE() AND TABLE_NAME='videos' AND INDEX_NAME='idx_hls_ready'
  ) THEN
    ALTER TABLE `videos` ADD INDEX `idx_hls_ready` (`hls_ready`);
  END IF;

  COMMIT;
END $$

DELIMITER ;
CALL migrate_videos_hls();
DROP PROCEDURE migrate_videos_hls;
/* ===================== 结束 ===================== */

```

##### 2,后端代码修改

首先配置相关的路径

```
  media:
    ffmpeg: "D:\FFmpegSoftware\ffmpeg-7.1.1-essentials_build\ffmpeg-7.1.1-essentials_build\bin/ffmpeg.exe"   # 本机安装的 ffmpeg.exe 绝对路径
    videoRoot: "D:/JavaPractice/wisdrivideoLinux/wisdriVideoLinux/"                 # 你保存源MP4的根目录（示例）
    hlsRoot: "D:/JavaPractice/wisdrivideoLinux/wisdriVideoLinux/save_video/hls/"
```

然后好像没了

直接打包即可









##### 3，前端代码修改

现有的播放页面

```vue
<template>
  <div>
    <!-- 用户头部 -->
    <!-- 用户头部 -->
    <header class="user-header">
      <div class="header-content">
        <!-- 左侧：logo -->
        <div class="left-section">
          <div class="logo">
            <img :src="require('@/assets/logoword.png')" alt="logo" class="logo-img" />
          </div>
        </div>

        <!-- 中间：页面标题（已整体注释掉，不显示） -->
        <!-- 
        <div class="middle-section">
          <h1>中冶南方BIM讲堂</h1>
          <p class="subtitle">WISDRI&nbsp;&nbsp;BIM&nbsp;&nbsp;ACADEMY</p>
        </div>
        -->

        <!-- 右侧：用户信息和退出登录按钮 -->
        <div class="right-section">
          <div class="user-info">
            <span class="welcome-text">欢迎您，{{ username }}</span>
          </div>
          <el-button class="home-button button" @click="goHome">首页</el-button>
          <el-button class="logout-button button" @click="handleLogout">退出登录</el-button>
        </div>
      </div>
    </header>

   <div class="page-content">
    <!-- 播放窗口 -->
   <div class="main-content">
      <!-- 播放窗口 -->
      <div class="video-player-container" v-if="selectedVideo">
        <div class="title-row">
          <h1 class="title-main">课程详情</h1>
          <h2 class="title-sub">正在播放：{{ selectedVideo.title }}</h2>
        </div>

        <!-- 绑定 videoUrl -->
        <video ref="videoPlayer" controls :src="videoUrl" width="100%" height="auto" controlsList="nodownload" />
      </div>

      <!-- 视频列表 -->
      <div class="video-list-container">
        <h1 class="video-title">视频列表</h1>

        <!-- 添加分割线 -->
        <div class="divider"></div>

        <ul class="video-list">
          <li v-for="video in videos" :key="video.id" class="video-item">
            <div class="video-details">
              <a 
                href="javascript:void(0);" 
                @click="selectVideo(video)" 
                class="video-link" 
                :class="{'playing': selectedVideo && selectedVideo.id === video.id}">
                {{ video.title }}
              </a>
              <span class="video-duration">{{ video.durationFormatted }}</span>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>

    <!-- 页面底部 -->
    <footer class="page-footer">
        <img :src="require('@/assets/wisdrilogo.png')" alt="logo" class="footer-logo" />
        <span class="footer-text">中冶南方钢铁公司工程数字化(BIM)中心</span>
    </footer>

  </div>
</template>

<script setup>
import { useRouter, useRoute } from 'vue-router';
import { ref, onMounted } from 'vue';
import Request from '@/utils/request';
// 顶部脚本处新增
import Hls from 'hls.js';


const router = useRouter();
const route = useRoute();
const username = ref('');
const videos = ref([]);

// 当前选中的视频
const selectedVideo = ref(null);
// 播放器视频地址
const videoUrl = ref('');
const baseUrl = process.env.VUE_APP_API_URL;

/**
 * 统一生成播放 URL 的函数
 * 👉 现在：保持直链
 * 👉 以后：只改这里，比如去请求签名接口，返回假URL
 */
const getPlayUrl = (filePath) => {
  const norm = String(filePath || '')
    .replace(/\\/g, '/')   // 反斜杠 -> 正斜杠
    .replace(/^\.\/?/, ''); // 去掉开头 ./ 或 .\
  return `${baseUrl}/${norm}`;
};

onMounted(async () => {
  const storedUsername = sessionStorage.getItem('username');
  const token = sessionStorage.getItem("token");
  if (storedUsername && token) {
    username.value = storedUsername;
  } else {
    username.value = storedUsername || "未登录";
    return router.push("/login");
  }

  const categoryId = route.params.categoryId;
  if (!categoryId) {
    console.error('categoryId 不存在');
    return;
  }

  try {
    const response = await Request.get(`/user/Home/${categoryId}/videos`);

    // 初始化视频列表
    videos.value = response.data.map((video) => ({
      ...video,
      durationFormatted: '加载中...'
    }));

    console.log(videos.value);

    // 获取视频时长（用 getPlayUrl）
    videos.value.forEach(video => {
      const videoElement = document.createElement('video');
      videoElement.preload = 'metadata';
      videoElement.src = getPlayUrl(video.filePath);

      videoElement.onloadedmetadata = () => {
        video.durationFormatted = formatDuration(videoElement.duration);
        videoElement.remove(); // 释放临时 video
      };
      videoElement.onerror = () => {
        video.durationFormatted = '未知';
        videoElement.remove();
        videoElement.remove();
      };
    });

    // 默认播放第一个视频
    if (videos.value.length > 0) {
      selectVideo(videos.value[0]);
    }
  } catch (error) {
    console.error('加载视频数据失败:', error);
  }
});

// 选择视频并显示播放器（用 getPlayUrl）
const selectVideo = (video) => {
  selectedVideo.value = video;
  videoUrl.value = getPlayUrl(video.filePath);
};

// 格式化视频时长
const formatDuration = (duration) => {
  if (typeof duration !== 'number' || isNaN(duration) || duration <= 0) {
    return '未知';
  }
  const minutes = Math.floor(duration / 60);
  const seconds = Math.floor(duration % 60);
  return `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
};


const handleLogout = () => {
  sessionStorage.removeItem('username');
  localStorage.removeItem('token');
  router.push('/login');
};

const goHome = () => {
  router.push('/');
};
</script>



```

修改后的分片上传：

```vue
<template>
  <div>
    <!-- 用户头部 -->
    <header class="user-header">
      <div class="header-content">
        <!-- 左侧：logo -->
        <div class="left-section">
          <div class="logo">
            <img :src="require('@/assets/logoword.png')" alt="logo" class="logo-img" />
          </div>
        </div>

        <!-- 右侧：用户信息和退出登录按钮 -->
        <div class="right-section">
          <div class="user-info">
            <span class="welcome-text">欢迎您，{{ username }}</span>
          </div>
          <el-button class="home-button button" @click="goHome">首页</el-button>
          <el-button class="logout-button button" @click="handleLogout">退出登录</el-button>
        </div>
      </div>
    </header>

    <div class="page-content">
      <div class="main-content">
        <!-- 播放窗口 -->
        <div class="video-player-container" v-if="selectedVideo">
          <div class="title-row">
            <h1 class="title-main">课程详情</h1>
            <h2 class="title-sub">正在播放：{{ selectedVideo.title }}</h2>
          </div>

          <video ref="videoPlayer" controls width="100%" height="auto" controlsList="nodownload"></video>
        </div>

        <!-- 视频列表 -->
        <div class="video-list-container">
          <h1 class="video-title">视频列表</h1>
          <div class="divider"></div>

          <ul class="video-list">
            <li v-for="video in videos" :key="video.id" class="video-item">
              <div class="video-details">
                <a
                  href="javascript:void(0);"
                  @click="selectVideo(video)"
                  class="video-link"
                  :class="{'playing': selectedVideo && selectedVideo.id === video.id}"
                >
                  {{ video.title }}
                </a>
                <span class="video-duration">{{ video.durationFormatted }}</span>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <!-- 页面底部 -->
    <footer class="page-footer">
      <img :src="require('@/assets/wisdrilogo.png')" alt="logo" class="footer-logo" />
      <span class="footer-text">中冶南方钢铁公司工程数字化(BIM)中心</span>
    </footer>
  </div>
</template>

<script setup>
import { useRouter, useRoute } from 'vue-router';
import { ref, onMounted, onBeforeUnmount } from 'vue';
import Request from '@/utils/request';
import Hls from 'hls.js';

const router = useRouter();
const route = useRoute();
const username = ref('');
const videos = ref([]);

// 选中与播放器
const selectedVideo = ref(null);
const videoPlayer = ref(null);
let hls = null;

// 基础地址（用于把 /hls/... 变成绝对 URL）
const baseUrl = (process.env.VUE_APP_API_URL || '').replace(/\/$/, '');

/** 把相对路径补成绝对 URL（用于 m3u8） */
function absolutize(u) {
  if (!u) return u;
  if (/^https?:\/\//i.test(u)) return u;       // 已是绝对
  return `${baseUrl}${u.startsWith('/') ? '' : '/'}${u}`;
}

/** 销毁 hls 实例，切换视频前调用 */
function destroyHls() {
  if (hls) {
    try { hls.destroy(); } catch (_) {}
    hls = null;
  }
}

/** 请求后端准备切片并返回 m3u8 地址 */
async function prepareM3u8(videoId) {
  const res = await Request.post('/hls/prepare', null, { params: { videoId } });
  return res?.m3u8 || res?.data?.m3u8 || res?.data?.data?.m3u8;
}

/** 用 m3u8 播放（Safari 原生 / 其它浏览器用 hls.js） */
async function playM3u8(m3u8Url) {
  const el = videoPlayer.value;
  if (!el) return;

  // 停止旧播放 & 清 src
  try { el.pause(); } catch {}
  el.removeAttribute('src');
  try { el.load(); } catch {}

  destroyHls();

  const url = absolutize(m3u8Url); // 把 /hls/... 变成 http://IP:PORT/hls/...

  if (el.canPlayType('application/vnd.apple.mpegurl')) {
    // Safari / iOS
    el.src = url;
    try { await el.play(); } catch {}
  } else if (Hls.isSupported()) {
    // Chrome/Firefox/Edge
    hls = new Hls({ maxBufferLength: 30 });
    hls.loadSource(url);
    hls.attachMedia(el);

    // 播放就绪
    hls.on(Hls.Events.MANIFEST_PARSED, () => {
      el.play().catch(() => {});
    });

    // ★ 用 LEVEL_LOADED 拿总时长（不触发 mp4 请求）
    hls.on(Hls.Events.LEVEL_LOADED, (_evt, data) => {
      const dur = data?.details?.totalduration;
      if (selectedVideo.value && typeof dur === 'number' && !Number.isNaN(dur)) {
        selectedVideo.value.durationFormatted = formatDuration(dur);
      }
    });

    hls.on(Hls.Events.ERROR, (_ev, data) => {
      console.error('HLS error:', data);
    });
  } else {
    // 老浏览器兜底
    el.src = url;
    try { await el.play(); } catch {}
  }
}

onMounted(async () => {
  const storedUsername = sessionStorage.getItem('username');
  const token = sessionStorage.getItem('token');
  if (storedUsername && token) {
    username.value = storedUsername;
  } else {
    username.value = storedUsername || '未登录';
    return router.push('/login');
  }

  const categoryId = route.params.categoryId;
  if (!categoryId) {
    console.error('categoryId 不存在');
    return;
  }

  try {
    const response = await Request.get(`/user/Home/${categoryId}/videos`);

    // 初始化列表；时长先占位，不再去直连 mp4 读取 metadata
    videos.value = response.data.map((video) => ({
      ...video,
      durationFormatted: '—',
    }));

    if (videos.value.length > 0) {
      await selectVideo(videos.value[0]);
    }
  } catch (error) {
    console.error('加载视频数据失败:', error);
  }
});

/** 选择视频并播放（仅 HLS，不触发任何 mp4 请求） */
const selectVideo = async (video) => {
  selectedVideo.value = video;
  // 切换后先重置时长显示
  selectedVideo.value.durationFormatted = '—';

  try {
    const m3u8 = await prepareM3u8(video.id);
    if (!m3u8) throw new Error('后端未返回 m3u8 地址');
    await playM3u8(m3u8);
  } catch (e) {
    console.error('播放失败：', e);
  }
};

function formatDuration(duration) {
  if (typeof duration !== 'number' || Number.isNaN(duration) || duration <= 0) return '—';
  const minutes = Math.floor(duration / 60);
  const seconds = Math.floor(duration % 60);
  return `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
}

onBeforeUnmount(() => {
  destroyHls();
});

const handleLogout = () => {
  sessionStorage.removeItem('username');
  localStorage.removeItem('token');
  router.push('/login');
};

const goHome = () => {
  router.push('/');
};
</script>

```





## 2025.9.26部署

#### 1，ffmpeg虚拟机安装

已经完成 

路径E:\FFmpegSoftware\ffmpeg-7.1.1-essentials_build\ffmpeg-7.1.1-essentials_build\bin

#### 2，后端部署

##### 1，修改后端配置文件



##### 2，修改后端ffmpeg的文件夹位置--已修改

```java
  media:
    ffmpeg: "E:/FFmpegSoftware/ffmpeg-7.1.1-essentials_build/ffmpeg-7.1.1-essentials_build/ffmpeg.exe"   # 本机安装的 ffmpeg.exe 绝对路径
    videoRoot: "E:/wisdriVideo/save_video/"                 # 你保存源MP4的根目录（示例）
    hlsRoot: "E:/wisdriVideo/save_video/hls/"
```

##### 3，后端数据库修改

记得备份数据库



已经备份数据库并且把videos修改了



##### 4，数据库接口修改

关于无法看视频的问题已经修改了  根据上面的备注



#### 3，前端部署

前端搞清楚要修改哪些部分

1，两个登录页面 和两个播放页面



2，vue.config  得自己观察有没有区别了



3，request.js!  对就是这个要修改



4，缺图片资源和依赖

已经安装



Hubei Sheng Wuhan Shi Jiangxia Qu Minzu Dadao 336 Hao Ping An Yuyuan 7-3-101





# 10.14  用户学习情况统计功能

## 学员视角

1，首页展示”已发布学习任务“→ 查看任务 →播放学习任务里的视频 → 全部达标后任务变“已完成”。

2，任务内页显示：视频清单（标题｜我的完成度%｜播放按钮）与任务整体进度条（已达标数/总数）

3，进入某视频 → 自动续播到上次进度（可选）

## 管理者视角

1，创建任务（选视频、选对象和截止时间） → 在管理页面看整体完成率与到期情况 。

2，任务详情：创建完任务后，可查看用户的具体完成情况



#### 从开发者角度，已经满足的需求：

**学习进度统计** —— 用户能看到每个视频的完成度和任务整体进度。

**学习任务管理** —— 管理者能布置任务，用户能完成，系统能自动判定完成状态。

**结果查看** —— 管理端能查看各任务、各用户的学习情况。





# 进度规划：

## 管理端（先做）——功能模块

1. **创建学习任务**
   - 填写任务名称，选择要学习的视频，选择参与对象（勾选或导入名单），可设置截止时间。
   - 发布后生成任务（内容固定，不改；需要变更就新建一个任务）。
2. **任务列表（总览）**
   - 展示每个任务的：对象人数、已开始人数、完成率、截止时间、当前状态（未开始/进行中/已截止）。
   - 支持按状态或截止时间排序/筛选，方便快速定位要关注的任务。
3. **任务详情（名单视图）**
   - 展示该任务下**每位学员**的：已完成视频数/总数、当前进度%、最后学习时间、状态（未开始/进行中/已完成/逾期未完）。
   - 支持按状态筛选“未完成/逾期”人群，便于线下提醒。
4. **导出（可放第二步）**
   - 从任务详情导出当前名单与进度，便于报表汇总或对接考核。

> 管理端完成口径：**视频达标（如≥90%）计入任务进度；任务内所有视频达标即“已完成”。**

------

## 学员端（后做）——功能模块

1. **我的任务列表**
   - 展示分配给我的所有任务，按“进行中 / 已完成 / 即将到期（有截止时）”分区显示。
   - 每个任务卡片显示整体进度（已达标数/总数）与截止时间。
2. **任务详情页**
   - 展示该任务的视频清单；每个视频显示我的完成度（%）和“去学习”按钮。
   - 顶部显示任务完成条件与我的整体进度，便于判断还差哪些。
3. **播放页学习提示**
   - 进入视频时提示该视频所属任务与达标线；达标后给出“返回任务/继续下一个”的引导。
4. **续播（可选，最后做）**
   - 回到同一视频时自动定位到上次离开的时间点；任务页同步最新进度。



**结论**：方案在数据库与实现上都**成立**，按此执行没问题。

**唯一要你拍板的选项**：**“历史观看是否计入任务进度”**。默认建议“计入”，实现最简、体验最好。



# 10.15  开会后开发总结



方案完全改变，优先学习如何通过域账户来获取员工所有信息！

然后再根据这个结果去获取表



目前功能：用户访问统计

1，管理员界面：查询学习记录

2，用户：自己的学习进度




http://192.168.145.201:46500/ 





### 1，LDAP拉取数据+后期一年维护一次？

LDAP本机测试总结：

```shell
C:\Users\14088>ldapsearch -H ldap://192.168.200.14:389 -x -s base -b "" namingContexts
# extended LDIF
#
# LDAPv3
# base <> with scope baseObject
# filter: (objectclass=*)
# requesting: namingContexts
#

#
dn:
namingContexts: DC=wisdri,DC=com
namingContexts: CN=Configuration,DC=wisdri,DC=com
namingContexts: CN=Schema,CN=Configuration,DC=wisdri,DC=com

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1

C:\Users\14088>
```

匿名账户可以连接

ldapwhoami -H ldap://192.168.200.14:389 -x -D "14088@wisdri.com" -W



已经拉取成功，两个表格emp_user和另一个用于复制的过度表

其中拥有了公司所有人的数据









#### 涂博士发现问题汇总：

首页：

1，视频封面大小不一：`手动上传同一尺寸，尝试过后台统一切效果不好`



管理端

**1，视频分类新建时出现上次名称：修改成空白**

**2，无法修改编辑名称：没有选择视频分类，修改成默认原分类**

3，上传失败后进度条不重置：刷新页面，后续寻找其他方法解决

4，视频展示顺序：目前按照默认顺序，调研b站后发现也是同一方式，暂不修改

**5，视频上传失败：原因未知，后续需要排查**







### 2，管理页面--学习进度统计

创建了



user_video_progress和user_video_session

```mysql
-- ① 用户-视频汇总进度（页面主要查这个）
CREATE TABLE IF NOT EXISTS user_video_progress (
  id                BIGINT PRIMARY KEY AUTO_INCREMENT,
  employee_code     VARCHAR(64) NOT NULL,     -- emp_user.employee_code
  video_uid         CHAR(32) NOT NULL,        -- 直接用你的视频主键字段名
  watched_seconds   INT NOT NULL DEFAULT 0,
  last_position_sec INT NOT NULL DEFAULT 0,
  progress_pct      DECIMAL(5,2) NOT NULL DEFAULT 0.00,
  completed         TINYINT NOT NULL DEFAULT 0,   -- 达到阈值(默认90%)即置1
  first_watch_at    DATETIME NULL,
  last_watch_at     DATETIME NULL,
  UNIQUE KEY uk_user_video (employee_code, video_uid),
  KEY idx_user (employee_code),
  KEY idx_video (video_uid)
  -- 外键可选：FOREIGN KEY (employee_code) REFERENCES emp_user(employee_code)
  -- 外键可选：FOREIGN KEY (video_uid)     REFERENCES video(video_uid)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- ②（可选）会话流水，用于审计与重算（前端每10~15s上报心跳）
CREATE TABLE IF NOT EXISTS user_video_session (
  id                BIGINT PRIMARY KEY AUTO_INCREMENT,
  employee_code     VARCHAR(64) NOT NULL,
  video_uid         CHAR(32) NOT NULL,
  session_id        VARCHAR(64) NOT NULL,     -- 前端uuid
  started_at        DATETIME NOT NULL,
  ended_at          DATETIME NULL,
  seconds_played    INT NOT NULL DEFAULT 0,
  last_position_sec INT NOT NULL DEFAULT 0,
  source_ip         VARCHAR(64) NULL,
  user_agent        VARCHAR(200) NULL,
  KEY idx_session (session_id),
  KEY idx_user_video_time (employee_code, video_uid, started_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;


这里下面还加了很多约束和主键  因为之前的是用工号当主键 不太行
-- 1. 新增列（先允许为 NULL，便于回填）
ALTER TABLE user_video_progress
    ADD COLUMN user_id BIGINT NULL AFTER employee_code;

-- 2. 回填 user_id
UPDATE user_video_progress p
    JOIN emp_user u ON u.employee_code = p.employee_code
SET p.user_id = u.id;

-- 3. 加索引/约束（先并存双唯一键，便于灰度）
ALTER TABLE user_video_progress
    ADD INDEX idx_uvp_user (user_id),
    ADD CONSTRAINT fk_uvp_user FOREIGN KEY (user_id) REFERENCES emp_user(id);

-- 4. 新的唯一约束（user_id, video_uid）
ALTER TABLE user_video_progress
    ADD UNIQUE KEY uk_userid_video (user_id, video_uid);

-- 5. 确认无空值后，再收紧为 NOT NULL（检查：SELECT COUNT(*) FROM user_video_progress WHERE user_id IS NULL）
ALTER TABLE user_video_progress
    MODIFY COLUMN user_id BIGINT NOT NULL;

DESC user_video_progress;

UPDATE user_video_progress p
    JOIN emp_user u ON u.employee_code = p.employee_code
SET p.user_id = u.id
WHERE p.user_id IS NULL;

```





####  一、数据库设计结构总结

##### 1. `emp_user` — 用户信息表（源自LDAP）

| 字段                                      | 含义                                       |
| ----------------------------------------- | ------------------------------------------ |
| `id`                                      | 用户唯一主键（INT，自增）✅（用于统一索引） |
| `employee_code`                           | 工号（也可以是账号，早期主键）             |
| `name`, `company_name`, `department_name` | 基本身份信息                               |
| `status`                                  | 是否在职（1有效 / 0无效）                  |

------

##### 2. `user_video_progress` — 用户对每个视频的学习进度（核心表）

| 字段                               | 含义                           |
| ---------------------------------- | ------------------------------ |
| `user_id`                          | 对应 `emp_user.id`，主用字段 ✅ |
| `video_uid`                        | 视频唯一标识                   |
| `watched_seconds`                  | 已累计观看时长（秒）           |
| `last_position_sec`                | 上次播放停止的时间点（秒）     |
| `progress_pct`                     | 当前完成百分比（如 92.5）      |
| `completed`                        | 是否完成（90% 默认阈值）       |
| `first_watch_at` / `last_watch_at` | 首次/最近学习时间              |

> ✅ 唯一约束：`UNIQUE (user_id, video_uid)`，每人每视频一条记录

------

##### 3. `user_video_session`（可选） — 播放会话记录

| 字段                      | 含义                         |
| ------------------------- | ---------------------------- |
| `session_id`              | 前端生成的 UUID              |
| `started_at` / `ended_at` | 播放会话时间区间             |
| `seconds_played`          | 本次播放累计秒数             |
| `last_position_sec`       | 本次最后播放位置             |
| `source_ip`, `user_agent` | 可选审计信息（防刷、防作弊） |

------

####  二、接口层 Mapper 总结

##### `LearnWriteMapper.java`

用于 **更新学习进度**（服务于心跳上报）

- `getProgress(...)`：查询当前进度（用于判断是否前进）
- `insertProgress(...)`：首次播放该视频时插入记录
- `updateProgress(...)`：每次前进或完成更新
- `upsertSession(...)`：写入播放会话（可选，统计用）

------

##### `LearnReadMapper.java`

用于 **查询学习进度/历史/未完成视频列表**

- `getVideoProgress(...)`：查询某用户某视频的学习情况
- `listHistory(...)`：查询最近学习的视频（有记录）
- `listNotStarted(...)`：还未开始学习的视频
- `listUnfinished(...)`：学习进度<100%的视频

------

####  三、整体逻辑流程总结

> 👨‍🏫「每个用户的每个视频学习进度」是核心逻辑，表现在：

1. 用户通过浏览器进入视频播放页面，播放 `.m3u8`
2. 前端每 10~15 秒通过 API 上报当前播放时间点 + 播放秒数
3. 后端判断是**首次学习**还是**进度更新**，调用对应 SQL 写入
4. 后端还可以记录会话流水，供后续分析和复盘
5. 查询端口可根据 `user_id` 快速获取学习列表、进度详情、未学清单等





#### 四，后端部分

这里选择了前端将登录之后的**empcode--工号**返回 然后后端再查询的方式

所以前端需要给出相应回复，在请求的时候返回信息

```
headers: {
  'X-Username': sessionStorage.getItem('username') || 'test-emp'
}
```



##### （1）heartbeat功能理解



先给videos表补充视频总时长字段duration_time

已经完成



该接口已经调通 可以写入数据了





##### （2）视图创建 自动执行汇总

视图不管了 反正可以查看和复制 知道有就行了





#### 五，前端心跳

在前端添加心跳功能

并修改数据库



#### 六，后端统计和导出excel  ---11.4

```
@Api(tags = "通用导出模块")
@RestController
@RequestMapping("/api/auth/export")
```

但是这个接口只是测试用 导出excel好像不是用这个



#### 七，前端总结展示--初步

前端条件(JSON)
     ↓
Controller（/query, /export）
     ↓
Service（分页查询 / 导出Excel）
     ↓
Mapper（动态SQL聚合）
     ↓
MyBatis XML（user_video_session + progress + category）
     ↓
MySQL数据库







### **`deptsWithTime`**

> 📊 功能：统计某个分类下各部门的整体学习情况。

**输入参数：**

- `categoryId`（分类 ID）
- `startDate` / `endDate`（时间段，用于时长统计）
- `deptName`（可模糊过滤部门）

**输出结果字段（`DeptSummaryDTO`）：**

| 字段             | 含义                        |
| ---------------- | --------------------------- |
| `deptName`       | 部门名称                    |
| `totalUsers`     | 部门总人数                  |
| `completedUsers` | 完成所有视频的用户数        |
| `completionRate` | 完成率（完成人数 / 总人数） |
| `totalHours`     | 学习总时长（小时）          |
| `avgHours`       | 平均学习时长（小时）        |



### **`deptUsersWithTime`**

> 👤 功能：查询指定部门中每个用户在该分类下的学习详情。

**输入参数：**

- `categoryId`（分类 ID）
- `deptName`（部门名，精确匹配）
- `startDate` / `endDate`（时间段，可选）
- `page`, `pageSize`（分页）

**输出结果字段（`DeptUserDetailDTO`）：**

| 字段               | 含义                           |
| ------------------ | ------------------------------ |
| `employeeCode`     | 员工工号                       |
| `userName`         | 员工姓名                       |
| `totalVideos`      | 分类视频总数                   |
| `completedVideos`  | 用户已完成视频数               |
| `completionRate`   | 用户完成率                     |
| `completedFlag`    | 是否已完成全部视频（1/0）      |
| `periodTotalHours` | 该时间段内的总学习时长（小时） |







#### 自定义查询---前端分两行展示按钮和查询的版本

```
<template>
    <div class="learn-statistics-page">
        <h1 class="page-title">自定义查询</h1>

        <!-- 一、查询 + 表格 -->
        <section class="query-section">
            <div class="query-bar">
                <el-form :inline="true" :model="query" size="small">
                    <el-form-item label="部门">
                        <el-select v-model="query.dept" placeholder="请选择部门" style="width: 180px">
                            <el-option v-for="item in deptList" :key="item.name" :label="item.name"
                                :value="item.name" />
                        </el-select>
                    </el-form-item>
                    <el-form-item label="姓名">
                        <el-input v-model="query.userName" placeholder="输入姓名"></el-input>
                    </el-form-item>
                    <el-form-item label="工号">
                        <el-input v-model="query.employeeCode" placeholder="输入工号"></el-input>
                    </el-form-item>
                    <el-form-item label="分类">
                        <el-select v-model="query.category" placeholder="选择视频分类" style="width: 180px">
                            <el-option v-for="item in categoryList" :key="item.value" :label="item.label"
                                :value="item.value" />
                        </el-select>
                    </el-form-item>

                    <el-form-item label="时间范围">
                        <el-date-picker v-model="query.dateRange" type="daterange" unlink-panels
                            :disabled-date="disableFuture" start-placeholder="开始日期" end-placeholder="结束日期"
                            format="YYYY-MM-DD" />
                    </el-form-item>
                    <el-form-item>
                    </el-form-item>
                </el-form>
                <div class="query-actions">
                    <el-button type="primary" @click="searchData">查询</el-button>
                    <el-button @click="resetQuery">重置</el-button>
                    <el-button type="success" @click="exportExcel">导出 Excel</el-button>
                </div>
            </div>

            <div class="table-wrapper">
                <el-table :data="tableData" stripe border style="width: 100%;" :fit="true">
                    <el-table-column prop="employeeCode" label="工号" min-width="100" />
                    <el-table-column prop="userName" label="姓名" min-width="120" />
                    <el-table-column prop="deptName" label="部门" min-width="150" />
                    <el-table-column prop="categoryName" label="分类" min-width="150" />
                    <el-table-column prop="videoCount" label="学习视频数" min-width="120" />
                    <el-table-column prop="completedCount" label="已完成" min-width="100" />
                    <el-table-column prop="durationMinutes" label="学习时长(分钟)" min-width="140" />
                    <el-table-column prop="avgCompletionRate" label="平均完成率(%)" min-width="140" />
                </el-table>
                <div v-if="tableData.length === 0" class="no-data-tip">
                    暂无查询结果，请调整条件后重试
                </div>
            </div>
        </section>

        <!-- 二、统计概览 -->
        <section class="charts-section">
            <h2>学习统计概览</h2>
            <div class="charts-grid">
                <div class="chart-card" ref="deptChart"></div>
                <div class="chart-card" ref="videoChart"></div>
                <div class="chart-card" ref="userChart"></div>
            </div>
        </section>
    </div>
</template>

<script setup>
import { ref, reactive, onMounted, nextTick } from 'vue'
import * as echarts from 'echarts'
import Request from '@/utils/request'

/* ---------------- 查询与表格 ---------------- */
const query = reactive({
    dept: '',
    userName: '',   // 必须加这个
    employeeCode: '',
    category: '',
    dateRange: []
})
const categoryList = ref([]) // 改成空数组，后面动态加载
const tableData = ref([])
const deptList = ref([])

/** 加载真实视频分类（你后面可替换成自己的接口） */


function disableFuture(time) {
    return time.getTime() > Date.now()
}


async function getCategoryList() {
    try {
        const res = await Request.get('/admin/category')
        if (res.data && res.data.categories) {
            categoryList.value = res.data.categories.map(c => ({
                label: c.name,
                value: c.id
            }))
            console.log(' 分类列表加载成功:', categoryList.value)
        } else {
            console.warn(' 返回数据结构异常:', res)
            categoryList.value = []
        }
    } catch (err) {
        console.error(' 获取视频分类失败:', err)
        categoryList.value = []
    }
}

/** 查询后端多条件接口 */
async function searchData() {
    try {
        const payload = {
            dept: query.dept || null,
            userName: query.userName || null,     // 必须加这一行
            employeeCode: query.employeeCode || null,
            categoryId: query.category || null,
            startDate: query.dateRange?.[0] || null,
            endDate: query.dateRange?.[1] || null,
            pageNum: 1,
            pageSize: 30
        }
        const res = await Request.post('/learn/custom/query', payload)
        tableData.value = res.data?.rows || []
    } catch (e) {
        console.error('查询失败:', e)
        tableData.value = []
    }
}

function resetQuery() {
    query.dept = ''
    query.employeeCode = ''
    query.category = ''
    query.dateRange = []
    tableData.value = []
}

async function exportExcel() {
    try {
        const payload = {
            dept: query.dept || null,
            employeeCode: query.employeeCode || null,
            categoryId: query.category || null,
            startDate: query.dateRange?.[0] || null,
            endDate: query.dateRange?.[1] || null
        }
        const blob = await Request.post('/learn/custom/export', payload, { responseType: 'blob' })
        const url = window.URL.createObjectURL(new Blob([blob]))
        const a = document.createElement('a')
        a.href = url
        a.download = '学习统计结果.xlsx'
        a.click()
        window.URL.revokeObjectURL(url)
    } catch (e) {
        console.error('导出失败:', e)
    }
}

/* ---------------- ECharts 区域 ---------------- */
const deptChart = ref(null)
const videoChart = ref(null)
const userChart = ref(null)
const TOP_N = 5

function mapAggregateFilter(list, nameKey, valueKey) {
    const acc = new Map()
    for (const it of list || []) {
        const name = (it?.[nameKey] ?? '').toString().trim() || '未分配'
        const rawVal = Number(it?.[valueKey])
        const val = Number.isFinite(rawVal) ? rawVal : 0
        acc.set(name, (acc.get(name) || 0) + val)
    }
    return Array.from(acc.entries()).map(([name, value]) => ({ name, value }))
}

function topN(arr, n = TOP_N) {
    return [...arr].sort((a, b) => b.value - a.value).slice(0, n)
}

function setEmptyChart(chart, title) {
    chart.clear()
    chart.setOption({
        title: { text: `${title}（无数据）`, left: 'center' },
        xAxis: { type: 'category', data: [] },
        yAxis: { type: 'value' },
        series: []
    })
}

/** 渲染单个图表（封装） */
function renderChart(refDom, optionBuilder, title) {
    if (!refDom.value) return
    const chart = echarts.getInstanceByDom(refDom.value) || echarts.init(refDom.value)
    chart.showLoading()
    optionBuilder(chart)
    setTimeout(() => chart.resize(), 300)
    window.addEventListener('resize', chart.resize)
    chart.hideLoading()
}

async function loadDeptChart() {
    renderChart(deptChart, async chart => {
        try {
            const res = await Request.get('/learn/summary/deptAll');

            // 不做任何换算，直接拿 totalMinutes
            const raw = (res.data || []).map(it => ({
                deptName: it.deptName ?? '未分配',
                totalMinutes: Number(it.totalMinutes ?? 0)
            }));

            const dataTop = topN(mapAggregateFilter(raw, 'deptName', 'totalMinutes'));
            if (dataTop.length === 0) return setEmptyChart(chart, '部门学习时长');

            chart.setOption({
                title: { text: '部门学习时长', left: 'center' },
                tooltip: { trigger: 'axis' },
                grid: { left: 40, right: 20, bottom: 30, top: 50 },
                xAxis: { type: 'category', data: dataTop.map(d => d.name) },
                yAxis: { type: 'value', name: '分钟' },
                series: [{ type: 'bar', data: dataTop.map(d => d.value) }]
            });
        } catch (e) {
            console.error('部门学习时长加载失败:', e);
            setEmptyChart(chart, '部门学习时长');
        }
    }, '部门学习时长');
}


/* 2) 视频热度排行 */
async function loadVideoChart() {
    renderChart(videoChart, async chart => {
        try {
            const res = await Request.get('/learn/summary/video')
            const raw = (res.data || []).map(it => {
                const title = it.title ?? '未知视频'
                const val = Number(it.total_seconds ?? 0)
                const value = Math.round((val / 60) * 100) / 100 // ✅ 这个接口仍然返回秒，继续除以60
                return { title, value }
            })

            const dataTop = topN(mapAggregateFilter(raw, 'title', 'value'))
            if (dataTop.length === 0) return setEmptyChart(chart, '视频热度排行')

            chart.setOption({
                title: { text: '视频热度排行', left: 'center' },
                tooltip: { trigger: 'item', formatter: '{b}: {c} 分钟' },
                series: [{ type: 'pie', radius: '70%', data: dataTop }]
            })
        } catch (e) {
            console.error('视频热度排行加载失败:', e)
            setEmptyChart(chart, '视频热度排行')
        }
    }, '视频热度排行')
}

async function loadUserChart() {
    renderChart(userChart, async chart => {
        try {
            const res = await Request.get('/learn/summary/userRank');

            // 不做换算
            const raw = (res.data || []).map(it => ({
                name: it.userName ?? it.empCode ?? '未知用户',
                totalMinutes: Number(it.totalMinutes ?? 0)
            }));

            const dataTop = topN(mapAggregateFilter(raw, 'name', 'totalMinutes'));
            if (dataTop.length === 0) return setEmptyChart(chart, '用户学习排行榜');

            chart.setOption({
                title: { text: '用户学习排行榜', left: 'center' },
                tooltip: { trigger: 'axis' },
                grid: { left: 40, right: 20, bottom: 30, top: 50 },
                xAxis: { type: 'category', data: dataTop.map(d => d.name) },
                yAxis: { type: 'value', name: '分钟' },
                series: [{ type: 'bar', data: dataTop.map(d => d.value) }]
            });
        } catch (e) {
            console.error('用户学习排行加载失败:', e);
            setEmptyChart(chart, '用户学习排行榜');
        }
    }, '用户学习排行榜');
}

async function loadDeptList() {
    const res = await Request.get("/learn/progressbyD/dept-list");
    deptList.value = res.data || res || [];  // 
}



/* ---------------- 初始化 ---------------- */
onMounted(async () => {
    await nextTick()
    await getCategoryList()

    // 延迟执行确保容器有宽高
    setTimeout(() => {
        loadDeptChart()
        loadVideoChart()
        loadUserChart()
        loadDeptList()
    }, 300)
})
</script>



<style scoped>
.learn-statistics-page {
    padding: 30px;
    background-color: #f7f9fc;
    min-height: 100vh;
    font-family: "Microsoft YaHei", Arial, sans-serif;
}

/* ---------------- 页面标题 ---------------- */
.page-title {
    text-align: center;
    margin-bottom: 28px;
    font-size: 26px;
    font-weight: 700;
    color: #2b3d63;
    letter-spacing: 1px;
}

/* ---------------- 查询区域 ---------------- */
.query-section {
    background: #fff;
    padding: 20px 28px;
    border-radius: 14px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.04);
    transition: box-shadow 0.3s;
}

.query-section:hover {
    box-shadow: 0 6px 14px rgba(0, 0, 0, 0.06);
}

/* 查询表单保持一行排列 */
.query-bar .el-form {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    row-gap: 10px;
    /* ✔ 增加行距更好看 */
}

.query-bar .el-form-item {
    margin-right: 20px;
    margin-bottom: 0;
    flex-shrink: 0;
}

.el-input,
.el-select,
.el-date-picker {
    width: 200px;
}

.query-actions {
    width: 100%;
    display: flex;
    justify-content: flex-end;
    /* 按钮靠右 */
    margin-top: 10px;
    gap: 12px;
    /* 按钮之间有空隙 */
}


/* 按钮样式 */
.el-button {
    border-radius: 6px;
    font-weight: 500;
    letter-spacing: 0.3px;
}

.el-button--primary {
    background-color: #357ef1;
    border-color: #357ef1;
    color: #fff;
}

.el-button--primary:hover {
    background-color: #2e6cda;
}

.el-button--success {
    background-color: #2ecc71;
    border-color: #2ecc71;
    color: #fff;
}

.el-button--success:hover {
    background-color: #27ae60;
}

/* ---------------- 表格区 ---------------- */
.table-wrapper {
    margin-top: 16px;
    max-height: 420px;
    overflow-y: auto;
}

.el-table {
    width: 100%;
    font-size: 14px;
    border-radius: 10px;
    overflow: hidden;
    --el-table-header-bg-color: #f3f6fa;
}

.el-table th {
    background-color: #f3f6fa !important;
    color: #333;
    font-weight: 600;
    text-align: center;
}

.el-table td {
    text-align: center;
    height: 44px;
}

/* 无数据时的提示样式 */
.no-data-tip {
    text-align: center;
    color: #888;
    padding: 15px 0;
    font-size: 14px;
}

/* ---------------- 图表区 ---------------- */
.charts-section {
    margin-top: 40px;
    background: #fff;
    padding: 24px;
    border-radius: 14px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.04);
}

.charts-section h2 {
    text-align: center;
    font-size: 20px;
    color: #2b3d63;
    font-weight: 600;
}

.charts-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(340px, 1fr));
    gap: 24px;
    margin-top: 24px;
}

.chart-card {
    height: 340px;
    border: 1px solid #e8edf3;
    border-radius: 12px;
    background: #fafbfe;
    box-shadow: inset 0 0 4px rgba(0, 0, 0, 0.03);
    transition: transform 0.2s ease;
}

.chart-card:hover {
    transform: translateY(-3px);
}
</style>

```

