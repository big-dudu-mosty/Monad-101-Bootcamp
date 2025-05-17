完成课程的实操后，请思考如何监听合约事件；当有别人购买了像素格子的时候，如何及时通过监听事件更新 UI ? 请提交示例代码
1、合约部分

//合约声明的购买事件，参数：格子的位置，账号，格子颜色
event SquarePurchased(uint8 idx, address buyer, uint256 color); 

//购买格子
function buySquare(uint8 idx, uint256 color) public payable { 
      //一些逻辑与参数校验...
     //设置颜色值
     squares[idx] = color;
     // 触发购买事件 
     emit SquarePurchased(idx, msg.sender, color);
}

2、前端部分
   //web3对象初始化，以及通过ABI获取合约
   web3 = new Web3(window.ethereum);
   contract = new web3.eth.Contract(contractABI, "0xd9145CCE52D386f254917e481eB44e9943F39138");
   // 监听合约的购买事件
   contract.events.SquarePurchased()
   .on('data', (event) => {
    console.log('新格子被购买:', event.returnValues);
    const { idx, color } = event.returnValues;
    // 更新UI
    const pixel = grid.children[idx];
    if (pixel) {
      pixel.style.backgroundColor = `#${parseInt(color).toString(16)}`;
    }
    }).on('error', (error) => {
      console.error('事件监听错误:', error);
   });


    //点击购买按钮的click监听
   connectButton.addEventListener('click', () => {
        try {
          buySquare();
        } catch (e) {
          console.log(e);
        }
      });

    async function buySquare() {
      const cellIndex = selectedCell.id;
      const color = parseInt(selectedColor.replace('#', ''), 16);
      console.log(cellIndex, color);
      //购买
      const price = web3.utils.toWei('0.001', 'ether');
      const tx = await contract.methods.buySquare(cellIndex, color).send({ from: userAccount, value: web3.utils.toWei("0.01", "ether") });
      console.log(tx);
       //获取各种信息，刷新界面UI
      getSquares();
    }
