using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace wpfCashFlow
{
    class PermuteArray
    {      
        List<int[]> arrPermutations = null;
        List<string> arrOfSets = null;
        int[] arrPos2 = new int[] { 1, 2, 3, 4, 5, 6, 7, 8, 9,10};
        int nextItemPos = 0;
        int factor = 0;
        int tick = 0;        

        public List<string> PermuteArr()
        {
            //definitions of arrays
            int[] arrPos = new int[] { 1, 2, 3, 4, 5, 6, 7, 8, 9,10};
            //List<int> arrPos2 = new List<int> { };                                 
            //go backward - 1 and then increment items + 1 replace
            arrPermutations = new List<int[]> { };
            arrOfSets = new List<string> { };
            //start permutting -1
            factor = factorial(arrPos.Count() - 1);
            for (int i = 0; i < arrPos.Count(); i++)
            {
                if(i > 0)
                {
                   Swap(arrPos2, 0, i);
                   int x = 0;
                   foreach (int ap in arrPos2)
                   {
                     arrPos[x] = ap;
                     x += 1;
                   }
                   tick = 0;
                }                                
                Permute(arrPos, 0);                
            }
            return arrOfSets;
        }
        private void Permute(int[] arrPos, int arrItem)
        {
            int x = arrPos.Count() - 2;
            int y = arrPos.Count() - 1;            
            arrItem += 1;
            // arrPos2[arrItem]
            for (int j = arrItem+1; j <= arrPos2.Count()+1; j++) {
                
                if (arrItem < x && tick < factor)
                {
                    //Moving down until 
                    Permute(arrPos, arrItem);                    
                    bool bChange = true;
                    while (bChange == true)
                    {
                        bChange = false;
                        for (int ap = arrItem; ap < arrPos.Count()-1; ap++)
                        {
                            if (arrPos[ap + 1] < arrPos[ap])
                            {
                                bChange = true;
                                Swap(arrPos, ap, ap + 1);
                            }
                        }
                    }
                }
                else if (tick < factor) {                    
                    Swap(arrPos, x, y);
                    Console.WriteLine(GetResult(arrPos));
                    Swap(arrPos, x, y);
                    Console.WriteLine(GetResult(arrPos));
                    nextItemPos = arrItem - 1;
                    int lt = GetLastItem(arrPos2, arrPos, arrItem);
                    if (lt == 0)
                    {
                        break;
                    }
                    else {
                        Swap(arrPos, arrItem - 1, lt);
                    }                    
                }
            }
            arrItem -= 1;
            if (arrItem > 0) {
                int ni = GetNextItem(arrPos, arrItem);
                if (ni > 0) {
                    Swap(arrPos, arrItem - 1, ni);
                }                
            }            
        }
        //int[]
        private void Swap(int[] arrPos, int arrItem, int arrItemPos)
        {
            int i = 0;
            i = arrPos[arrItem];
            arrPos[arrItem] = arrPos[arrItemPos];
            arrPos[arrItemPos] = i;
            //return arrPos;
        }

        private int GetLastItem(int[] arrPos2, int[] arrPos, int arrItem)
        {
            int i = 0;            
            for (int j = nextItemPos; j < arrPos2.Count(); j++)
            {
                if (arrPos[j] > arrPos[nextItemPos] && j > nextItemPos)
                {
                    i = arrPos[j];
                    break;
                }
            }

            if (i != 0)
            {                
                for (int j = arrItem - 2; j < arrPos.Count(); j++)
                {
                    if (arrPos[j] == i)
                    {
                        i = j;
                        break;
                    }
                }             
            }
            return i;            
        }
        private string GetResult(int[] arrPos)
        {
            string result = "";
            foreach (int x in arrPos)
            {
                result += x.ToString() + ";";
            }
            tick += 1;
            arrOfSets.Add(result);
            return result;
        }
        private int factorial(int x) {
            int res = 1;
            for (int i = 1; i <= x; i++) {                
                res = i * res;
            }            
            return res;
        }
        private int GetNextItem(int[] arrPos, int arrItem) {
            int i = 0;
            for (int j = arrItem; j < arrPos.Count(); j++)
                {
                    if (arrPos[j] > arrPos[arrItem - 1] && i == 0 || arrPos[j] < arrPos[i] && arrPos[j] > arrPos[arrItem - 1])
                    {
                        i = j;
                    }
                }
            return i;
        }
    }
}