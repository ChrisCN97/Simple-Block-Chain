# Simple-Block-Chain
By C#

using System.Collections;  
using System;  
using static System.Console;  
  
  
//简易区块链  
namespace test  
{     
    //区块  
    public class Block  
    {  
        //基本属性：上一个区块的Hash值，交易信息，Nonce变量，当前区块的Hash  
        private int previousBlockHashCode;  
        private string transactionInfo;  
        private string nonce;  
        private int blockHashCode;  
  
        //构造函数  
        public Block()  
        {  
            transactionInfo = "the Creation";  
            previousBlockHashCode = 0;  
            blockHashCode = 0;  
        }  
  
        public Block(string tranInfo,int previousBlockHash)  
        {  
            transactionInfo = tranInfo;  
            previousBlockHashCode = previousBlockHash;  
        }  
  
        //重载GetHashCode函数  
        public override int GetHashCode()  
        {  
            nonce = DateTime.Now.ToFileTimeUtc().ToString() + transactionInfo + previousBlockHashCode;  
            return nonce.GetHashCode();  
        }  
  
        public int BlockHashCode  
        {  
            get { return blockHashCode; }  
            set { blockHashCode = value; }  
        }  
        public string TransactionInfo  
        {  
            get { return transactionInfo; }  
        }  
        public int PreviousBlockHashCode  
        {  
            get { return previousBlockHashCode; }  
        }  
        public string Nonce  
        {  
            get { return nonce; }  
        }  
    }  
  
    //区块链  
    public class Chain: DictionaryBase  
    {  
        //重载添加方法  
        public void Add(Block newBlock)  
        {  
            Dictionary.Add(newBlock.BlockHashCode, newBlock);  
        }  
          
        public Chain()  
        {  
            WriteLine("Create a BlockChain.");  
            this.Add(new Block());  
        }  
  
        //文字索引  
        public Block this[int hash]  
        {  
            get { return (Block)Dictionary[hash]; }  
        }  
  
        //通过Hash遍历输出区块链  
        public void GetChainInfo(Block nowBlock)  
        {  
            WriteLine("\n\nPut out the BlockChain: ");  
            Block p = nowBlock;  
            do  
            {  
                WriteLine(p.TransactionInfo);  
                p = this[p.PreviousBlockHashCode];  
            } while (p.BlockHashCode != 0);  
            WriteLine(p.TransactionInfo);  
        }  
    }  
  
    //广播当前Hash  
    public class Broadcast  
    {  
        public static int hashNow;  
    }  
  
    //挖矿  
    public class Mining  
    {  
        //获得Hash值  
        public static void GetHash(Block b)  
        {  
            WriteLine("Wait...");  
            int hash;  
            while (true)  
            {  
                hash = b.GetHashCode();  
                int remainder = hash % 50;//难度设置  
                if (remainder == 22)  
                {  
                    WriteLine("Mining succeed.");  
                    break;  
                }   
            }  
            Broadcast.hashNow = hash;  
            b.BlockHashCode = hash;  
        }   
          
        //挖矿程序  
        public static void Mine(Chain chain,Block block)  
        {  
            WriteLine("Start to get HashCode.");  
            GetHash(block);  
            chain.Add(block);  
        }  
    }  
  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            Block testBlock = new Block("transaction no.1", 0);  
            Chain testChain = new Chain();  
            Mining.Mine(testChain, testBlock);  
            testChain.GetChainInfo(testBlock);  
        }  
    }  
}
