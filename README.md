# DSA_Project
Knapsack Problem 0-1
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace knapsack
{
    class LinkedList
    {
        public Node start;
        public int Lenght;
        public LinkedList()
        {
            Lenght = 0;
            start = null;
        }
        public void InsertAtBeg(string na, int we, int pro, int rat)
        {
            Node n = new Node(na, we, pro, rat);
            if (start == null)
            {
                start = n;
            }
            else
            {
                n.next = start;
                start = n;
            }
        }
        public void DeleteFromBeg()
        {
            if (start != null)
            {
                start = start.next;
            }
        }
        public void InsertAtEnd(string na, int we, int pro, int rat)
        {
            Node temp = start;
            Node n = new Node(na, we, pro, rat);
            if (start == null)
                start = n;
            else
            {
                while (temp.next != null)
                {
                    temp = temp.next;
                }
                temp.next = n;
            }
        }
        public void DeleteFromEnd()
        {
            Node temp = start;
            if (start == null)
            { }
            else
            {
                while (temp.next.next != null)
                {
                    temp = temp.next;
                }
                temp.next = null;
            }
        }
        public Node Search(string obj)
        {
            Node temp = start;
            while ((temp.next != null) || (temp.objectname == obj))
            {
                if (temp.objectname == obj)
                    return temp;
                else
                    temp = temp.next;
            }
            return null;
        }
        public void InserAfterVal(string val, string na, int we, int pro, int rat)
        {
            Node node = new Node(na, we, pro, rat);
            Node n = Search(val);
            if (n != null)
            {
                node.next = n.next;
                n.next = node;
            }
        }
        public void DeleteVal(string val)
        {
            if (start.objectname == val)
            {
                start = start.next;
            }
            else
            {
                Node n = Search(val);
                if (n != null)
                {
                    Node temp = start;
                    try
                    {
                        while ((temp.next) != n)
                        {
                            temp = temp.next.next;
                        }
                        temp.next = temp;
                    }
                    catch (Exception e)
                    {
                        temp.next = null;
                    }
                }
            }
        }
        public void Insert(int index, string na, int we, int pro, int rat)
        {
            Node node = new Node(na, we, pro, rat);
            Node n = start;
            int count = 0;
            while ((n != null) || count == index - 1)
            {
                if (count == index - 1)
                {
                    node.next = n.next;
                    n.next = node;
                    break;
                }
                else
                { n = n.next; count++; }
            }
        }
    }
    class Node
    {
        public int weight, profit, ratio;
        public string objectname;
        public bool status;

        public Node next;
        public Node()
        {
        }
        public Node(string na, int we, int pro, int rat)
        {
            objectname = na; weight = we; profit = pro;
            ratio = rat; status = false;
        }
        public Node(string na, int we, int pro, int rat, Node n)
        {
            objectname = na; weight = we; profit = pro;
            ratio = rat; status = false;
            next = n;
        }
    }
    class Program
    {
        static int nof = 0, wei = 0;
        static void Main(string[] args)
        {
            LinkedList list = new LinkedList();
            Console.Write("Enter No Of Object: ");
            nof = int.Parse(Console.ReadLine());
            Console.Write("Enter Max Weight: ");
            wei = int.Parse(Console.ReadLine());
            for (int i = 0; i < nof; i++)
            {
                Console.Write("Object Name: ");
                string name = Console.ReadLine();
                Console.Write("Enter Profit of Object: ");
                int profit = int.Parse(Console.ReadLine());
                Console.Write("Enter Weight of Object: ");
                int weight = int.Parse(Console.ReadLine());
                int ratio = profit / weight;
                list.InsertAtEnd(name, weight, profit, ratio);
                Console.WriteLine("***{0}",ratio);
            }
            Node temp = list.start;
            while (temp != null)
            {
                temp.status = false;
                temp = temp.next;
            }
            KnapSackPro(list);
            Console.WriteLine("Type here");
            KnapSackWeight(list);
            Console.ReadKey();
        }
        static double weight = 0, profit = 0, ratio = 0, remaingweight = 0;
        public static void KnapSackPro(LinkedList list)
        {
            remaingweight = wei;
        A:
            Node temp = list.start; int large = 0; Node tem = list.start;
        while (temp != null)
        {
            if (!temp.status)
            { tem = temp; break; }
            temp = temp.next;
        }
            while (temp != null)
            {
                if (large <= temp.profit && !temp.status)
                { large = temp.profit; tem = temp; }
                temp = temp.next;
            }
            tem.status = true;
            if (tem.weight != 0)
            {
                if (weight + tem.weight <= wei&&!tem.status)
                {
                    weight += tem.weight; profit += tem.profit; remaingweight -= tem.weight;
                    Console.WriteLine("{0} Loaded", tem.objectname);

                    if (wei != weight && remaingweight != 0)
                        goto A;
                }
                else
                {

                    tem.status = false;
                    temp = list.start; large = 0; tem = list.start;
                    ratio = temp.ratio;
                    while (temp != null)
                    {
                        if (ratio <= temp.ratio && !temp.status)
                        { ratio = temp.ratio; tem = temp; }
                        temp = temp.next;
                    }
                    weight += remaingweight; profit += remaingweight * tem.ratio; remaingweight = 0;
                    Console.WriteLine("{0} Loaded", tem.objectname);

                }
            }
            else
            {
                tem.status = false;
                temp = list.start; large = 0; tem = list.start;
                ratio = temp.ratio;
                while (temp != null)
                {
                    if (ratio <= temp.ratio && !temp.status)
                    { ratio = temp.ratio; tem = temp; }
                    temp = temp.next;
                }
                weight += remaingweight; profit += remaingweight * tem.ratio; remaingweight = 0;
                Console.WriteLine("{0} Loaded", tem.objectname);

            }
            temp = list.start;
            Console.WriteLine("The Max Profit is: " + profit);
            Console.WriteLine("The remaing weight is: " + remaingweight);
        }
        public static void KnapSackWeight(LinkedList list)
        {
            profit = 0; weight = 0;
        A:
            remaingweight = wei; Node tem = list.start; Node temp = list.start;
            temp = list.start; int small = temp.weight;
            while (temp != null)
            {
                if (!temp.status)
                { tem = temp; break; }
                temp = temp.next;
            }
            while (temp != null)
            {
                if (small >= temp.weight && !temp.status)
                { small = temp.weight; tem = temp; }
                temp = temp.next;
            }
            tem.status = true;
            if (tem.weight != 0)
            {
                if (weight + tem.weight <= wei&&!tem.status)
                {
                    weight += tem.weight; profit += tem.profit; remaingweight -= tem.weight;
                    Console.WriteLine("{0} Loaded", tem.objectname);

                    if (wei != weight && remaingweight != 0)
                        goto A;
                }
                else
                {
                    tem.status = false;
                    temp = list.start; small = 0; tem = list.start;
                    ratio = temp.ratio;
                    while (temp != null)
                    {
                        if (ratio <= temp.ratio && !temp.status)
                        { ratio = temp.ratio; tem = temp; }
                        temp = temp.next;
                    }
                    weight += remaingweight; profit += remaingweight * tem.ratio; remaingweight = 0;
                    Console.WriteLine("{0} Loaded", tem.objectname);

                }
            }
            else
            {
                tem.status = false;
                temp = list.start; small = 0; tem = list.start;
                ratio = temp.ratio;
                while (temp != null)
                {
                    if (ratio <= temp.ratio && !temp.status)
                    { ratio = temp.ratio; tem = temp; }
                    temp = temp.next;
                }
                weight += remaingweight; profit += remaingweight * tem.ratio; remaingweight = 0;
                Console.WriteLine("{0} Loaded",tem.objectname);
            }
            temp = list.start;
            Console.WriteLine("The Max Profit is: " + profit);
            Console.WriteLine("The remaing weight is: " + remaingweight);
        }
    }
}
