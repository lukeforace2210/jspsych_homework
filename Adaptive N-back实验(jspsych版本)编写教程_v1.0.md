# 使用jspsych编写Adaptive N-Back实验

---

> 实验名称：Adaptive N-Back
> [参考文献](https://www.sciencedirect.com/science/article/abs/pii/S1053811905001424)：Harvey, P. O., Fossati, P., Pochon, J. B., Levy, R., LeBastard, G., Lehéricy, S., ... & Dubois, B. (2005). Cognitive control and brain resources in major depression: an fMRI study using the n-back task. Neuroimage, 26(3), 860-869.

---

## 目录

1. [实验介绍](#1. 实验介绍)
2. [文件结构](#2. 文件结构)
	- [index.html](#2.1 `index.html`)
	- [experiment.js](#2.2 `experiment.js`)
	- [config.json](#2.3 `config.json`)
	- [style.css](#2.4 `style.css`)
3. [详细代码解析](#3. 详细代码解析)
    - [index.html](#3.1 index.html)
		- [html文件代码解析](#3.2 代码解析)
    - [experiment.js](#3.3experiment.js)
    	- [js文件代码解析](#3.4 代码解析)
    - [config.json](#3.5config.json)
    	- [json文件代码解析](#3.6 代码解析)
    - [style.css](#3.7style.css)
    	- [css文件代码解析](#3.8 代码解析)
4. [运行实验](#4.运行实验)
5. [总结](#5.总结)

### 1. 实验介绍 

被试进行的是**<u>字母变体版的 "n-back "任务。</u>**这些任务要求保持和永久更新工作记忆中的相关信息。工作记忆中的负荷和心理操作通过三个复杂程度的**<u>==1/2/3-back==</u>**任务进行了调整。被试还进行了一项对照任务<u>*（0-back）*</u>，要求他们识别一个预先指定的字母。简单来说就是被试需要指出屏幕上出现的字母是否与之前出现的字母相一致。

### 2. 文件结构

一个jspsych实验想要正常运行，其一级文件夹目录中必须包含以下文件：

#### 2.1 `index.html`

`index.html`文件通常用于启动实验，是实验的入口点。它负责设置实验的基本结构，并引入所有必要的脚本和样式文件。内容包括：
1. 引入 jsPsych 库文件、插件文件、实验脚本和样式文件。
2. 设置实验的基本HTML结构。
3. 包含初始化和启动实验的代码。

#### 2.2 `experiment.js`

`experiment.js`文件是实验脚本文件，用于定义实验的具体逻辑和流程，它使用 jsPsych 库和插件来设置和运行实验。内容包括：
1. 定义实验的时间线和各个阶段。
2. 设置刺激呈现、响应记录和数据保存。
3. 实现实验条件和流程控制

#### 2.3 `config.json`

`config.json`文件通常用于存储实验配置参数和设置，以便于在实验运行时动态加载这些参数。这种做法有以下几个好处：
1. **分离数据与逻辑**：将实验参数和设置与实验逻辑分离，使代码更易于维护和阅读。
2. **灵活性**：可以轻松修改实验参数而无需更改代码。这样可以方便地进行实验条件的调整和重复试验。
3. **多条件实验**：对于需要运行多种条件或版本的实验，可以通过修改`config.json`文件来实现，而无需更改主代码。

`config.json`文件的具体内容取决于实验的需求，可能包括以下信息：
1. **实验参数**: 如试次数量、刺激呈现时间、间隔时间等。
2. 刺激路径: 图像、音频或视频文件的路径。
3. 被试信息: 参与者的ID、组别等。
4. 实验结构: 不同阶段或模块的顺序及相关参数。

#### 2.4 `style.css`

`style.css`文件用于定义实验的样式和布局，使实验界面更加美观和用户友好。内容包括：
1. 设置实验界面的字体、颜色和布局。
2. 自定义 jsPsych 元素的样式。
3. 添加动画和其他视觉效果。

### 3.  详细代码解析

#### 3.1 index.html

`index.html`是实验的入口文件，负责加载所有必要的脚本和样式表，并初始化实验。

为了编写Adaptive N-Back实验，其html文件如下所示：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Adaptive N-Back</title>

  <script src="https://www.naodao.com/public/experiment/libs/axios.min.js"></script>
  <script src="https://www.naodao.com/experiment/ajax/libs/jquery/2.2.0/jquery.min.js"></script>
  <script src="static/js/math.min.js"></script>
  
  <link rel="stylesheet" href="static/css/jspsych.css">
  <link rel="stylesheet" href="static/css/default_style.css">
  <link rel="stylesheet" href="style.css">
  
  <script src="static/js/math.min.js"></script>
  <script src="static/js/jspsych/jspsych.js"></script>
  <script src="static/js/jspsych/plugins/jspsych-text.js"></script>
  <script src="static/js/jspsych/poldrack_plugins/jspsych-poldrack-text.js"></script>
  <script src="static/js/jspsych/poldrack_plugins/jspsych-poldrack-instructions.js"></script>
  <script src="static/js/jspsych/poldrack_plugins/jspsych-attention-check.js"></script>
  <script src="static/js/jspsych/poldrack_plugins/jspsych-poldrack-single-stim.js"></script>
  <script src="static/js/jspsych/poldrack_plugins/jspsych-poldrack-categorize.js"></script>
  <script src="static/js/jspsych/plugins/jspsych-survey-text.js"></script>
  <script src="static/js/jspsych/plugins/jspsych-call-function.js"></script>
  <script src="static/js/utils/poldrack_utils.js"></script>
  <script src="experiment.js"></script>
  <script src="https://www.naodao.com/experiment/libs/jspsych5/naodao-2022-08.js"></script>
</head>

<body></body>
<script>
    $(document).ready(function () {
      jsPsych.init({
        timeline: jsPsych.plugins.NaoDao.mergeTrails(adaptive_n_back_experiment),
        display_element: "getDisplayElement",
        fullscreen: true,
      });
    });
</script>

</html>
```

#### 3.2 代码解析

- `<head>`部分包含了所有必要的脚本和样式文件的引用。
  - `<script src="..."></script>`标签用于加载外部JavaScript库和插件，包括`axios`、`jquery`、`math.min.js`、`jspsych`及其插件。
  - `<link rel="stylesheet" href="...">`标签用于加载外部CSS样式表，包括jsPsych的默认样式和自定义样式。
- `<body>`部分在加载完毕后会调用一个JavaScript函数，使用`$(document).ready()`确保在文档完全加载后执行初始化代码。
- `jsPsych.init`方法用于初始化实验。
  - `timeline`参数指定了实验的时间线。
  - `display_element`指定了显示实验内容的元素。
  - `fullscreen`设置实验全屏显示。

### 3.3 experiment.js

`experiment.js`是实验的核心文件，包含了实验的主要逻辑和流程控制。为了编写Adaptive N-Back实验，其js文件如下所示：

```javascript
/* ************************************ */
function evalAttentionChecks() {
  var check_percent = 1;
  if (run_attention_checks) {
    var attention_check_trials = jsPsych.data.getTrialsOfType('attention-check');
    var checks_passed = 0;
    for (var i = 0; i < attention_check_trials.length; i++) {
      if (attention_check_trials[i].correct === true) {
        checks_passed += 1;
      }
    }
    check_percent = checks_passed / attention_check_trials.length;
  }
  return check_percent;
}

function assessPerformance() {
  var experiment_data = jsPsych.data.getTrialsOfType('poldrack-single-stim');
  var missed_count = 0;
  var trial_count = 0;
  var rt_array = [];
  var rt = 0;
  var choice_counts = {};
  choice_counts[-1] = 0;
  choice_counts[32] = 0;
  for (var i = 0; i < experiment_data.length; i++) {
    if (experiment_data[i].possible_responses != 'none') {
      trial_count += 1;
      rt = experiment_data[i].rt;
      key = experiment_data[i].key_press;
      choice_counts[key] += 1;
      if (rt == -1) {
        missed_count += 1;
      } else {
        rt_array.push(rt);
      }
    }
  }
  var avg_rt = -1;
  if (rt_array.length !== 0) {
    avg_rt = math.median(rt_array);
  }
  var missed_percent = missed_count / experiment_data.length;
  var responses_ok = true;
  Object.keys(choice_counts).forEach(function (key, index) {
    if (choice_counts[key] > trial_count * 0.85) {
      responses_ok = false;
    }
  });
  credit_var = (missed_percent < 0.4 && avg_rt > 200 && responses_ok);
  jsPsych.data.addDataToLastTrial({ credit_var: credit_var });
}

var getInstructFeedback = function () {
  return '<div class = centerbox><p class = center-block-text>' + feedback_instruct_text + '</p></div>';
};

var randomDraw = function (lst) {
  var index = Math.floor(Math.random() * lst.length);
  return lst[index];
};

var record_acc = function (data) {
  var target_lower = data.target.toLowerCase();
  var stim_lower = curr_stim.toLowerCase();
  var key = data.key_press;
  if (stim_lower == target_lower && key == 37) {
    correct = true;
    if (block_trial >= delay) {
      block_acc += 1;
    }
  } else if (stim_lower != target_lower && key == 40) {
    correct = true;
    if (block_trial >= delay) {
      block_acc += 1;
    }
  } else {
    correct = false;
  }
  jsPsych.data.addDataToLastTrial({
    correct: correct,
    stim: curr_stim,
    trial_num: current_trial
  });
  current_trial = current_trial + 1;
  block_trial = block_trial + 1;
};

var update_delay = function () {
  var mistakes = base_num_trials - block_acc;
  if (delay >= 2) {
    if (mistakes < 3) {
      delay += 1;
    } else if (mistakes > 5) {
      delay -= 1;
    }
  } else if (delay == 1) {
    if (mistakes < 3) {
      delay += 1;
    }
  }
  block_acc = 0;
  current_block += 1;
};

var update_target = function () {
  if (stims.length >= delay) {
    target = stims.slice(-delay)[0];
  } else {
    target = "";
  }
};

var getStim = function () {
  var trial_type = target_trials.shift();
  var targets = letters.filter(function (x) { return x.toLowerCase() == target.toLowerCase() });
  var non_targets = letters.filter(function (x) { return x.toLowerCase() != target.toLowerCase() });
  if (trial_type === 'target') {
    curr_stim = randomDraw(targets);
  } else {
    curr_stim = randomDraw(non_targets);
  }
  stims.push(curr_stim);
  return '<div class = "centerbox"><div class = "center-text">' + curr_stim + '</div></div>';
};

var getData = function () {
  return {
    trial_id: "stim",
    exp_stage: "adaptive",
    load: delay,
    target: target,
    block_num: current_block
  };
};

var getText = function () {
  return '<div class = "centerbox"><p class = "block-text">

请根据当前字母是否与之前的字母相匹配来做出反应。如果匹配，按左箭头；如果不匹配，按下箭头。<br>按任意键继续。</p></div>';
};

/* 实验设置 */
var num_blocks = 20;
var base_num_trials = 20;
var control_num_trials = 3;
var experiment_len = base_num_trials * num_blocks;
var num_trials = base_num_trials + control_num_trials;
var block_acc = 0;
var delay = 1;
var curr_stim = '';
var stims = [];
var current_block = 0;
var current_trial = 0;
var target_trials = jsPsych.randomization.repeat(['target', 'nontarget'], base_num_trials / 2);
target_trials = target_trials.concat(jsPsych.randomization.repeat(['target', 'nontarget'], control_num_trials / 2));
target_trials = jsPsych.randomization.shuffle(target_trials);

/* 实验时间线 */
var instruction_node = {
  timeline: [{
    type: 'poldrack-instructions',
    pages: [
      '<div class = centerbox><p class = block-text>欢迎参加实验。请在接下来的任务中保持注意力集中。<br>按任意键继续。</p></div>',
      '<div class = centerbox><p class = block-text>在接下来的任务中，您需要根据当前字母是否与之前的字母相匹配来做出反应。如果匹配，按左箭头；如果不匹配，按下箭头。<br>按任意键继续。</p></div>',
      '<div class = centerbox><p class = block-text>任务将在接下来开始，请保持注意力集中并尽量不犯错误。<br>按任意键继续。</p></div>'
    ],
    allow_keys: true,
    data: {
      trial_id: "instruction"
    },
    show_clickable_nav: true,
    timing_post_trial: 1000
  }],
  loop_function: function (data) {
    for (i = 0; i < data.length; i++) {
      if (data[i].trial_type == 'instruction' && data[i].rt < 2000) {
        sumInstructTime += data[i].rt;
      }
    }
    if (sumInstructTime <= instructTimeThresh) {
      return true;
    } else if (sumInstructTime > instructTimeThresh) {
      return false;
    }
  }
};

var start_test_block = {
  type: 'poldrack-text',
  data: {
    trial_id: "start_test_block"
  },
  timing_response: 180000,
  text: '<div class = centerbox><p class = block-text>请在接下来的任务中保持注意力集中并尽量不犯错误。<br>按enter键开始。</p></div>',
  cont_key: [13],
  timing_post_trial: 1000
};

var trial = {
  type: 'poldrack-single-stim',
  stimulus: getStim,
  is_html: true,
  data: getData,
  choices: [37, 40],
  timing_post_trial: 0,
  timing_response: 1500,
  response_ends_trial: true,
  on_finish: record_acc
};

var feedback_block = {
  type: 'poldrack-text',
  data: {
    trial_id: "feedback_block"
  },
  timing_response: 180000,
  text: function () {
    if (block_trial >= base_num_trials) {
      update_delay();
    }
    return '<div class = centerbox><p class = block-text>本轮结束。<br>按enter键开始下一轮。</p></div>';
  },
  cont_key: [13],
  timing_post_trial: 1000
};

var adaptive_n_back_experiment = [];
adaptive_n_back_experiment.push(instruction_node);
adaptive_n_back_experiment.push(start_test_block);
for (var b = 0; b < num_blocks; b++) {
  for (var i = 0; i < num_trials; i++) {
    adaptive_n_back_experiment.push(trial);
  }
  adaptive_n_back_experiment.push(feedback_block);
}

/* 初始化实验 */
jsPsych.init({
  timeline: adaptive_n_back_experiment,
  display_element: 'jspsych-target',
  on_finish: function () {
    jsPsych.data.displayData();
  }
});
```

#### 3.4 代码解析

1. **函数定义**:
   - `evalAttentionChecks()`: 评估注意力检查的通过率。
   - `assessPerformance()`: 评估实验参与者的表现，包括反应时间、错误率和响应分布。
   - `getInstructFeedback()`: 返回指导反馈文本。
   - `randomDraw(lst)`: 从列表中随机抽取一个元素。
   - `record_acc(data)`: 记录参与者的反应准确性，并根据反应更新实验数据。
   - `update_delay()`: 根据参与者的表现调整实验难度（延迟）。
   - `update_target()`: 更新目标刺激。
   - `getStim()`: 生成当前刺激。
   - `getData()`: 返回当前试验的数据。
   - `getText()`: 生成任务介绍文本。

2. **变量定义**:
   - 定义了实验的参数（如块数、试验数）和全局变量（如当前刺激、目标刺激、反应准确性等）。

3. **时间线设置**:
   - `instruction_node`: 定义实验的指导节点，包含多个指导页面。
   - `start_test_block`: 定义测试开始块，显示开始测试的提示信息。
   - `trial`: 定义单个试验块，显示刺激并记录参与者的反应。
   - `feedback_block`: 定义反馈块，在每个块结束后显示反馈信息。

4. **实验初始化**:
   - `adaptive_n_back_experiment`: 定义整个实验的时间线，将各个块按顺序添加到时间线中。
   - `jsPsych.init`: 使用jsPsych初始化实验并设置时间线。

### 3.5 config.json

`config.json`文件包含实验的配置信息。为了编写Adaptive N-Back实验，其json文件如下所示：

```json
{
    "settings": {
        "experiment_name": "Adaptive N-Back",
        "author": "Author Name",
        "version": "1.0",
        "description": "An adaptive N-back task."
    },
    "experiment_parameters": {
        "num_blocks": 20,
        "base_num_trials": 20,
        "control_num_trials": 3
    },
    "stimuli": {
        "letters": ["b", "B", "d", "D", "g", "G", "t", "T", "v", "V"]
    },
    "feedback": {
        "correct_message": "Correct!",
        "incorrect_message": "Incorrect."
    }
}
```

#### 3.6 代码解析

1. `settings`

这个部分包含了实验的一些基本信息。

```json
"settings": {
    "experiment_name": "Adaptive N-Back",
    "author": "Author Name",
    "version": "1.0",
    "description": "An adaptive N-back task."
}
```

- **experiment_name**: 实验的名称。这在实验的介绍页面或报告中可以使用。
- **author**: 实验的作者或创建者。
- **version**: 实验的版本号，这对于版本控制和更新记录很有用。
- **description**: 实验的简短描述，说明实验的主要目的或任务类型。

2. `experiment_parameters`

这个部分定义了实验的一些参数，控制实验的结构和流程。

```json
"experiment_parameters": {
    "num_blocks": 20,
    "base_num_trials": 20,
    "control_num_trials": 3
}
```

- **num_blocks**: 实验中的区块数量。每个区块可以包含一系列试次。
- **base_num_trials**: 每个区块中的基本试次数量。这是进行 N-Back 任务的主要试次。
- **control_num_trials**: 控制试次数量，可能用于校准或检测参与者的基线水平。

3. `stimuli`

这个部分定义了实验中使用的刺激材料。在这个例子中，是一些字母。

```json
"stimuli": {
    "letters": ["b", "B", "d", "D", "g", "G", "t", "T", "v", "V"]
}
```

- **letters**: 刺激材料列表，这里是大小写的字母，用于 N-Back 任务中呈现给参与者。

4. `feedback`

这个部分定义了实验中提供给参与者的反馈信息。

```json
"feedback": {
    "correct_message": "Correct!",
    "incorrect_message": "Incorrect."
}
```

- **correct_message**: 当参与者回答正确时显示的信息。
- **incorrect_message**: 当参与者回答错误时显示的信息。

### 3.7 style.css

`style.css`文件定义了实验的样式。为了编写Adaptive N-Back实验，其css文件如下所示：

> 在编写当前实验的css文件时，使用了**flexbox语法**。CSS Flexbox（Flexible Box）是一种用于布局的CSS3模块，旨在提供更高效和更方便的方式来排列和对齐容器中的元素，尤其是当这些元素的尺寸是动态的或未知的（例如，响应式设计）。Flexbox布局模型允许你轻松地创建各种复杂的布局结构，并且在调整屏幕大小时保持一致。

```css
.centerbox {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.block-text {
  display: flex;
  justify-content: center;
  font-size: 24px;
  text-align: center;
}

.center-text {
  display: flex;
  justify-content: center;
  font-size: 36px;
  text-align: center;
}
```

#### 3.8 代码解析

1. `.centerbox`类使元素在垂直和水平居中显示。

- `display: flex`: 将该元素设为flex容器，启用Flexbox布局。
- `justify-content: center`: 在主轴（水平轴）上居中对齐子元素。
- `align-items: center`: 在交叉轴（垂直轴）上居中对齐子元素。
- `height: 100vh`: 设置元素的高度为视口高度的100%。这个属性确保该元素占据整个视口的高度，使其内容能够在垂直方向上居中对齐。

2. `.block-text`类设置块文本的字体大小和对齐方式。

- `display: flex`: 将该元素设为flex容器，使其可以使用Flexbox布局的对齐属性。
- `justify-content`: center: 在主轴（通常是水平轴）上居中对齐子元素。这里的子元素是文本内容。
- `font-size: 24px`: 设置字体大小为24像素，使文本更容易阅读。
- `text-align: center`: 在行内元素中水平居中对齐文本内容。

3. `.center-text`类设置中心文本的字体大小和对齐方式。

- `display: flex`: 将该元素设为flex容器，使其可以使用Flexbox布局的对齐属性。
- `justify-content`: center: 在主轴（通常是水平轴）上居中对齐子元素。这里的子元素是文本内容。
- `font-size: 36px`: 设置字体大小为36像素，使文本更加显眼和突出。
- `text-align: center`: 在行内元素中水平居中对齐文本内容。


### 4. 运行实验

1. 确保所有文件在同一目录下。
2. 打开`index.html`文件，实验将自动开始。

### 5. 总结

通过上述步骤，应该能够理解并复现一个基于jsPsych框架的心理学实验。