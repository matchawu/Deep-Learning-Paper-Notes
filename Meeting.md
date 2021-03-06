# Meeting

- **原本初版的idea&novelty**
    - idea
        - adversarial training
        - GCN
        - 網路學習擾動量
        - iterative產擾動
        - 更多鄰居資訊考量
- **建議可以修正的方向**
    - 手法比較偏attack
    - 缺乏structure上的考量
    - 防禦現有的graph attack手法，而非純FGSM
    - baselines應該使用SOTA
        - 例如在node classification的accuracy，應該不是直接跟原本GCN比較，而是後面提出的最新的方法
        - 例如defense上面，在adversarial training上最好的(有找到一些也大部分有更好，但沒有放上去)，或甚至不侷限在adverssarial training上面的方法
        - 對實驗的討論不夠多，結果不夠說服人
- **目前版本的idea&novelty**
    - 在node perturbation上
        - 使用原本三種方式對features產perturbation
    - 在structure perturbation上
        - 參考[Adversarial Attack on Graph Structured Data](https://arxiv.org/pdf/1806.02371.pdf) (透過DQN的方式找到兩個nodes去加減邊，以達到攻擊效果)
        - 預期做到：做DQN一樣選擇兩個點作為加減邊依據
            - state: 目前的graph
            - action: 分兩次各選一個點
            - reward: 更改後的graph去train之validation的accuracy
            - 作為adversarial training的方法
            - 選兩個點->翻轉邊->train GCN(adversarial trianing)->validation accuracy
            - 設定每次actions數量
- journal version論文準備進度
    - Introduction
        - 修改部分敘述
        - 待：contributions修改
    - Related Work
        - 依照reviewers意見，按照survey paper架構重寫一遍
        - cite更全面的graph attack & adversarial training papers
        - 提及：
            - graph embedding methods
            - adversarial attack on graphs
            - adversarial defense on graphs
        - 待：RL在graph方面的survey
    - Proposed Method
        - 原本的adversarial training方式
            - 網路學習擾動量
            - iterative產擾動
            - 更多鄰居資訊考量
        - 待：RL架構
            - flip k edges to enhance the accuracy/robustness of the node classifier
    - Experiments
        - 原本的實驗：
            - 分類準確度
                - classification accuracy
                - clean samples
            - 參數敏感性
                - iterative步數
                - 鄰居階層數量
                - 產生的擾動的penalty大小
            - 防禦能力
                - 針對FGSM攻擊，準確度的下降程度
        - 待：
            - 找問題：
                - gp再Citeseer上不夠好
                - gp的設定不夠精細：網路、penalty等
            - RL相關實驗
            - 對目前attack手法的防禦能力(*survey)
            - 和其他adversarial training的手法比較
- **實驗部分**
    - ![](https://i.imgur.com/Hkp5IHG.png) ![](https://i.imgur.com/WQxifKT.png)
        - 要再把像GraphSGAN的SBVAT、OBVAT以及latent版本的AT放進去表格
    - Dataset
        - citation networks * 3
        - large graph?
    - RL
        - ![](https://i.imgur.com/XPghyv6.png)
        - dqn的理解
        - code本身的理解(c++ & Python)
        - code classification修好了可以跑，但attack部分還不行，有cuda相容性問題，至今仍在解決中
        - 仍在改寫code中 T_________________T
            - 1) code本身cuda環境的問題(解決中)
            - 2) DQN的QNet從s2v(C++)改成GCN去建embedding
            - 3) reward設定改掉: acc下降越多越高->validation accuracy越高
    - Meeting feedbacks
        - RL state
            -  整張圖(每個node看到一樣的東西)or每個node看到自己的subgraph？
        - 何向南作為rule-based方法，我的作為更intellegent的方法
            - 一起講，避免被說太偏向attack
        - 安排除了RL寫出來以外的各項事情的順序
            - defense現有的attack方法
            - larger datasets 例如至少具有上萬個nodes 總共用4~5個datasets
            - baselines調整
                - AT手法->或更多的defense手法比較
                - clean acc和SOTA相比 而不是和GCN或更舊的方法比

