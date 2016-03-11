# cseHomework

import java.util.*;
import java.io.*;


public class HuffmanTree {

   private HuffmanNode nodeEnd;
   private HuffmanNode decoder; 
   
   public HuffmanTree(int [] count) {
      PriorityQueue<HuffmanNode> nodeQueue = new PriorityQueue<HuffmanNode>(count.length);
      for(int i = 0 ; i < count.length; i++) {
         if(count[i] > 0) {
            nodeQueue.add(new HuffmanNode(i, count[i]));
         }
      }
      nodeQueue.add(new HuffmanNode(256, 1));
      makeHuffman(nodeQueue);
   }
   
   private void makeHuffman(PriorityQueue<HuffmanNode> queue) {
      while(queue.size() > 1) {
         HuffmanNode curr = new HuffmanNode(0, 0);
         curr.left = queue.poll();
         curr.right = queue.poll();
         curr.frequency = curr.left.frequency + curr.right.frequency;
         queue.add(curr);
      }
      
      nodeEnd = queue.poll();
   }

   public void write(PrintStream output) {
      String beginningCode = "";
      write(nodeEnd, output, beginningCode);
   }
   
   private void write(HuffmanNode nodeRoot, PrintStream output, String codeString) {
      if(nodeRoot.character != 0) {
         output.println(nodeRoot.character);
         output.println(codeString);
      } else {
         write(nodeRoot.left, output, codeString + '0');
         write(nodeRoot.right, output, codeString + '1');
      }
   }
   
   public HuffmanTree(Scanner input) {
      decoder = new HuffmanNode(0,0);
      while(input.hasNextLine()) {
         int n = Integer.parseInt(input.nextLine());
         String code = input.nextLine();
         decodeHuffman(n, code);
      }
   }
   
   private void decodeHuffman(int character, String code){ 
      HuffmanNode temp = decoder;
      for(int i = 0; i < code.length(); i++) {
         if(code.charAt(i) == '1') {
            if(temp.right == null) {
               temp.right = new HuffmanNode(0,0);
               temp = temp.right;
            }
         } else {
            if(temp.left == null) {
               temp.left = new HuffmanNode(0,0);
               temp = temp.left;            
            }
         }
      }
      temp.character = character;
   
   } 
   
   public void decode(BitInputStream input, PrintStream output, int eof) {
      HuffmanNode temp = decoder;
      int i;
      while((i = input.readBit()) != -1) {
         if(i == 0) {
            temp = temp.left;
         } else {
            temp = temp.right;
         }
         
         if(temp.character == 0) {
            continue;
         }
         
         if(temp.character == eof) {
            break;
         }
      
         output.write(temp.character);
         temp = decoder;
      }
   
   }
   
   
   
   
   
   
   
   
public class HuffmanNode implements Comparable<HuffmanNode> { 

   public int character;
   public int frequency;
   public HuffmanNode left;
   public HuffmanNode right;

   public HuffmanNode (int character, int frequency) {
      this.character = character;
      this.frequency = frequency;
      this.left = null;
      this.right = null;
   }
   
   public int compareTo(HuffmanNode n) {
      return this.frequency - n.frequency;
   }
}
   
   
   
   


















}
