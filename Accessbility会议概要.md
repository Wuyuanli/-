What is Accessibility?
is a legal requirement in US, Canada, Europe
is to provide products that people with special needs.
（美国欧盟立法，要求软硬件需要方便有特殊需求的人群，如残疾人使用。）

具体条款：Section 508 and CVAA

尤其针对具有实时语音、视频、文字交流的软硬件产品，会增加一个CVAA法律约束，更加严格。

每个project项目的开发流程都要加入对accessbility的支持，比如敏捷迭代story grooming的时候，甚至是planning的时候针对每一个**带UI**的story都要去声明。

联想追求WCAG (Accessibility相关组织) 的double A标准，基本的东西就是4个单词：POUR。P是perceivable，可感知的，O是可操作的，U是可理解的，R是健壮的。

举例：提供替代文本，如字幕；键盘可操作；避免出现自动刷新或定时关闭；软件有足够的适应性，可以应对不同的读屏软件、页面布局。

（针对测试人员的AMP申请流程，主要用于法律豁免）

公司政策更新，即使是China Only的产品，无论软件还是硬件，也需要符合accessibility的要求，test case按美国的Section 508执行。

只有存在**UI交互**的部分才需要cover accessibility。