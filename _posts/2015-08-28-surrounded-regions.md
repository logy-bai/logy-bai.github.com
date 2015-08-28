---
layout: post
title: "leetcode No.130 Surrounded Regions"
description: ""
category: Algorithm
tags: [algorithm,]
---
{% include JB/setup %}

###No.130 题目：被包围的区域

	Given a 2D board containing 'X' and 'O', capture all regions surrounded by 'X'.
	A region is captured by flipping all 'O's into 'X's in that surrounded region.
	
	For example,
	
	X X X X
	X O O X
	X X O X
	X O X X
	
	After running your function, the board should be:

	X X X X
	X X X X
	X X X X
	X O X X


	给出一个二维的数组 board 里面包含字符 'X'和字符 'O',找到所有被字符 'X'包围的区域。把被包围区域的字符 'O'变成字符 'X'。

	例如：

	X X X X
	X O O X
	X X O X
	X O X X

	你的程序运行后，二维数组 board应该是：

	X X X X
	X X X X
	X X X X
	X O X X

###我的代码


```java
public class Solution {
	public class Location{
		int hei;
		int len;
		char value;
		Location(int x,int y,char val){
			hei = x;
			len = y;
			value = val;
		}
	}
	public boolean hasC(ArrayList<Location> list){
		Iterator<Location> iterator = list.iterator();
		while (iterator.hasNext()) {
			Location L = iterator.next();
			if(L.value == 'C'){
				return true;
			}
		}
		return false;
	}
    public void solve(char[][] board) {
		Queue<Location> queue = new LinkedList<Location>();
		ArrayList<Location> list = new ArrayList<Location>();
		int len = 0;
		int hei = 0;
		if (board == null || board.length == 0)
		    return;
        int height = board.length;
        if (board[0] == null)
            return;
        int length = board[0].length;
		for(len=0;len<length;len++){
			for(hei=0;hei<height;hei++){
				if(board[hei][len]=='O'){
						board[hei][len] = 'B';
					queue.offer(new Location(hei,len,board[hei][len]));
					while(!queue.isEmpty()){
						Location Lcur = queue.poll();
						if(Lcur.hei-1<0||Lcur.hei+1>=height||Lcur.len-1<0||Lcur.len+1>=length){
							board[Lcur.hei][Lcur.len] = 'C';
						}else{
							//up
							if(board[Lcur.hei-1][Lcur.len] != 'X'&&board[Lcur.hei-1][Lcur.len] != 'B'){
								queue.offer(new Location(Lcur.hei-1,Lcur.len,board[Lcur.hei-1][Lcur.len]));
								board[Lcur.hei-1][Lcur.len] = 'B';
							}
							//down
							if(board[Lcur.hei+1][Lcur.len] != 'X'&&board[Lcur.hei+1][Lcur.len] != 'B'){
								queue.offer(new Location(Lcur.hei+1,Lcur.len,board[Lcur.hei+1][Lcur.len]));
								board[Lcur.hei+1][Lcur.len] = 'B';
							}
							//left
							if(board[Lcur.hei][Lcur.len-1] != 'X'&&board[Lcur.hei][Lcur.len-1] != 'B'){
								queue.offer(new Location(Lcur.hei,Lcur.len-1,board[Lcur.hei][Lcur.len-1]));
								board[Lcur.hei][Lcur.len-1] = 'B';
							}
							//right
							if(board[Lcur.hei][Lcur.len+1] != 'X'&&board[Lcur.hei][Lcur.len+1] != 'B'){
								queue.offer(new Location(Lcur.hei,Lcur.len+1,board[Lcur.hei][Lcur.len+1]));
								board[Lcur.hei][Lcur.len+1] = 'B';
							}
						}
						list.add(new Location(Lcur.hei,Lcur.len,board[Lcur.hei][Lcur.len]));
					}
					//is queue1 contains Character 'C',if yes ,change all element in queue1 into 'C' 
					if(hasC(list)){
						Iterator<Location> iterator = list.iterator();
						while (iterator.hasNext()) {
							Location L2 = iterator.next();
							board[L2.hei][L2.len] = 'C';
						}
					}
					list.clear();
				}
			}
		}
		for(len=0;len<length;len++){
			for(hei=0;hei<height;hei++){
				if(board[hei][len]=='B'){
					board[hei][len] = 'X';
				}
				if(board[hei][len]=='C'){
					board[hei][len] = 'O';
				}
				
			}
		}
    }
}
```