# 什么是Prompt
Prompt可以被认为是我们每一次访问大模型的输入，而大模型给我们的返回结果则被称为 Completion。
# 如何设计高效的Prompt
设计高效 Prompt 的两个关键原则：编写清晰、具体的指令和给予模型充足思考时间。
* 输入的prompt需要清晰的表达需求，提供充足的上下文，以便语言模型进行理解；

    * 使用分隔符，可以选择用 ```，"""，< >， ，: 等
    * 使用结构化输出
    * 要求模型检查是否满足条件
    * 提供少量实例，"Few-shot" prompting（少样本提示），即在要求模型执行实际任务之前，给模型提供一两个参考样例，让模型了解我们的要求和期望的输出样式。


* 其次，如果Prompt是逐级推理的，也就是思维链的模式，也能提高结果的可靠性。
    
# 常用gpt聊天参数设置
## 输入参数(调用示例)
```
from openai import OpenAI
client = OpenAI(api_key=，base_url=)

completion = client.chat.completions.create(
  model="gpt-4o",
  messages=[
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Hello!"}
  ]
)

print(completion.choices[0].message)
```
client.chat.completions.create创建回复，其中参数包含有
* messages 类型：array  
一个列表，是一连串的对话信息输入，其中每个对象有两个必填字段：
    * role: the role of the messenger (either system, user, assistant or tool)
    * content: the content of the message (e.g., Write me a beautiful poem)
<span style="color:red">必填项
* model 类型：string  
所使用模型的的id，例如“gpt-3.5-0125”等，<span style="color:red">必填项
* max_tokens 类型：整数或null  
聊天完成过程中可以生成的最大令牌数。
* n 类型：整数或null，默认值为1  
为每条输入消息生成多少个聊天完成选项。注意，根据所有选项中生成的令牌数量付费。 1 保持 n 成本最小化。
* temperature 类型：数字或者null 默认值为1  
要使用的采样温度，介于 0 和 2 之间。较高的值（如 0.8）将使输出更加随机，而较低的值（如 0.2）将使其更加集中和确定
* top_p 类型：数字或null 默认值为1  
温度采样的替代方法，称为原子核采样，其中模型考虑概率质量top_p标记的结果。因此，0.1 表示仅考虑包含前 10% 概率质量的代币。<span style="color:red">但不要同时更改top_p与temperature.
## 输出参数
response的格式如下
```
{
  "id": "chatcmpl-123",
  "object": "chat.completion",
  "created": 1677652288,
  "model": "gpt-3.5-turbo-0125",
  "system_fingerprint": "fp_44709d6fcb",
  "choices": [{
    "index": 0,
    "message": {
      "role": "assistant",
      "content": "\n\nHello there, how may I assist you today?",
    },
    "logprobs": null,
    "finish_reason": "stop"
  }],
  "usage": {
    "prompt_tokens": 9,
    "completion_tokens": 12,
    "total_tokens": 21
  }
}
```

